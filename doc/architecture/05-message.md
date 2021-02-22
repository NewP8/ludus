# Message

nella realtà discorso telefono vs messaggi
Sistemi reattivi sono *message driven*

principalmente asincroni perchè: 
- permettono di fare altro lavoro intanto che altri rispondono linerano risorse
- riduce contese
- posso accodarli anche se destinatario non è disponibile

messaggi sincroni legati a necessità di dominio (ma consci delle conseguenze)

Sistemi distribuiti rendono difficile gestire le *transazioni* lunghe che coinvolgono tanti microservizi.


### Saga Pattern

molti step che devono essere completati altrimenti tutta transazione fallisce
in caso di fallimento compensazione di step fatti.

In caso di tiemout in sistemi distribuiti non sappiamo se problema è 
- in trasporto di richiesta
- in trasporto di risposta
- in elaborazione
  (compensazione in questi casi deve essere commutativa)

in saga difficile gestire fallimenti di transazioni

rollback vs compensazione (rollback cancello step mentre in compensazione rimane traccia)
in Saga meccanismo migliore è compensazione (idempotenti e se fallisce retry)
altrimenti vado avanti lo stesso e uso:
- retry
- riparto da cache
- error queue (processata da un'altra parte)

In Akka uso attori anche per gestire Saga

### Garanzia di consegna 

impossibilità di raggiungere un consenso se mezzo di comunicazione inaffidabile
**Two generals** 2 generali che devono attaccare :-)
* IMPOSSIBILE "exactly one delivery"
* At most once (no retry, no storage) default in Akka
* At least once (errore può avvenire in risposta perciò retry potrebbe mandare 2a volta etc., storage in sender)
  - per simulare exactly once il messaggio deve essere idempotente (o dideduplicazione con storage anche in receiver)
  default in Akka persistence e Lagom Broker API

## Messaging Pattern

Comunicazione fra microservizi
- point-to-point (accoppiamento diretto, osservabile)
- message broker - publish/subscribe (disaccoppiamento, complessità difficile da tenere sotto controllo)






