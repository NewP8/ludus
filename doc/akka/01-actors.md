# Akka

Toolkit per la creazione di applicazioni distribuite, concorrenti, fult tolerant e message driven
basato sul modello ad Attori (Actor Model)

Single thread illusion grazie al fatto che ogni attore garantisce di processare un messaggio alla volta.
(accesso a stato di Actor un thread alla volta, no lockm no sincronizzazioniM pià attori possono operare contemporaneamente)

Akka distribuito di default (non importa se attore è locale o remoto)

Fault tolerant perchè i fallimenti degli attori sono gestiti da un supervisore 
(chiamante non può x es barista fai il caffè poi chi ordina non sa cosa succede)

## Actor Model

1973 Carl Hewit 
modello di computazione (implementazioe Akka)
 - Processing (Behaviour)
 - Storage (State)
 - Communication (Messages)

In Akka tutte le computazioni avvengono attraverso gli attori.
Ogni attore ha un **indirizzo** e può
- creare un nuovo attore
- spedire un messaggio ad un altro attore
- modificare il proprio Behaviour per gestire i messaggi futuri

## Actor System

Insieme di attori che collaborano.

Organizzazione gerarchica (In cima system e user);
ogni attore ha un **path** (protocol :// actor-system-name @host.domain.com:5678 /user/child-name) e UUID.
Possono delegare lavoro a figli e supervisionare (gestire fallimenti)

L'actor system fornisce:

- Factory (actorRef)
- Dispatchers (e thread pool):
- Scheduler
- Configurazione
- Pub-Sub subscribe

## Actor

Ogni attore
- utilizzato tramite **ActorRef** (riferimento mai usata istanza effettiva) che permette di spedire messaggi
- ha una **Mailbox** dove i messaggi vengono accodati
- ha un **Dispatcher** che schedula elaborazione di messaggi 

Ogni attore riceve un messaggio alla volta(1) e dispatcher fa si che venga elaborato un messaggio alla volta(2)
Lo stato è interno e non va condiviso i messaggi immutabili.

Behaviour definito con funzione parziale receive: [Any,Unit] (Receive è alias) 
I msg non gestiti possono essere logati
Per permettere remoting Props deve essere serializzabile.

Ogni attore ha un **context** (implicit) che permette di:
- accedere a self, parent, children
- creare figli o stoppare attori
- Death watch
- ... altro (cambio Behaviour,..)

### comunicazione

`aRef ! "ciao"` 

Messaggi immutabili (in genere case class definiti in companion object) 
operazione asincrona.
Se si aspetta risposta di deve includere riferimento a mittente (implicit)
Ricevente può recuperare mittente con `sender()` (se non esiste più va *deadLetters*).
i messagi si possono schedulare (dopo t, ogni t).

### Testing Actors (akka-testkit)

* Unit test sincrono
`TestActorRef` permette di accedere a stato/behaviour dell'attore con `.underlyingActor`.
con TestActorRef i messaggi gestiti in modo sincrono (solo per test!)

* Integration test asincrono
Si crea mock di Actor con `TestProbe` poi si verifica comportamento con `.expectMsg`(asincrono).

### Lifecycle

```

 -(preStart)->  started     -*stop*->     stopped  -(postStop)->  terminated
                  ^      \\                   ^ 
                  |       \---*resume*--\\    |                                           
                  |        \--------------\   |                                           
                  |                       \\  |                                           
              restarting  <-(preRestart)-  faulty
                              *restart*
```    
al comando di stop (o se riceve messaggio *PoisonPill*):
- finisce di gestire messaggio
- sospende messagi in coda
- stoppa i figli
- attenda terminazione di figli e poi termina anche lui
  
(con messaggio *Kill* invece lancia *ActorKilledException*)

**DeathWatch** è facility che permette ad attore si osservare altro attore e ricevere messaggio *Terminated* quando termina.

### Handling Failure e Supervision

Esempio barista
non ritorna eccezione ma fallimento gestito da manager così barista può fare caffè
non previene fallimenti ma reagisco gestendoli.
Parental supervision fa si che in caso di fallimento:
- attore con fallimento sospende elaborazione messaggi
- ricorsivamente anche i figli
- il parent gestisce il fallimento (**supervisor strategy**: )

strategie:
- OneForOneStrategy - solo un child
- AllForOneStrategy - tutti i child

Decider sceglie x ogni Eccezione -> quale direttiva:
- Resume
- Restart (nuova istanza riprende con messaggi di precedente)
- Stop
- Escalate (passa a padre)

----
## Routers
strategie
...
## Dispatchers
Decide quale attore procede con elborazione messaggi
....
### modifica Behaviour
### ask pattern