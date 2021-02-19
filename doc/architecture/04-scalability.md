# Scalable Systems

consistency <--> availability  - impatto su scalabilità (rimane responsivo anche all'aumento del carico)
**performance**(latenza) vs **parallelismo** (carico)

Sistemi distribuiti (in space) 
- ci vuole tempo per trasmissione
- necessario consenso fra più parti
- lo stato può cambiare prima che comunicazione raggiunga tutti 

## Scalabilità

la realtà è **Eventual consistent**.
(casual consistency, sequential consistency,... , strong consistency)

Sistemi tradizionali strong consistent prevedono che tutti i nodi siano in accordo prima di presentare risposta.
(lock in database non distribuito)
lock comunque non garantisce strong consistency (ad es. seeksvil)
ma introduce **contention** (limita parallelismo).
Stato di **coerenze** deve essere condiviso con tutti nodi (costo può superare beneficio)
scalabilità lineare (rendimento x concorrenza) impossibile.

cosa si può fare:
- minimizzare contention (lock, transactions, operazioni bloccanti, sharding)
- minimizzare coherency delay (eventual consistency, ridurre comunicazione costruendo sistemi autonomi)

## Teorema CAP 

I sistemi distribuiti non possono fornire più di due dei tre seguenti:
- Availability
- Consistency
- Partition Tolerance: funziona anche se rete perde messaggi

soluzioni a partizione:
- AP write su entrambe le parti (poi da allineare)
- CP disabilito una parte
  
(CA - ognoro rete un nodo .. non ho problemi di rete)

### Application lever sharding

ogni entità stà in una shard/location (Entity = consistency boundaries).
Coordinator smista messaggi a Entità corretta.
(al primo messaggio indica in quale nodo stà l'entità, nei successivi non serve)
Bilanciamento delle entità diventa importante.

Shardg permette di isolare contetntion.
se suddiviso entità per id riduco contention.
Distribuendo gli shard nelle varie macchine posso diminuire il carico di lavoro
Entity mantengono strong consistency (entity processano un messaggio alla volta).

In caso di *fallimento* i messaggi vengono bufferizzati e se termino gracefully dopo
verranno rielaborati tutti.
Sharding è soluzione **CP** (guarda a consistenza).
Se terminazione violenta il resto del sistema non sa cosa succede e alcuni messaggi possono esssere persi.
Se shard va giu può essere migrato su altro nodo.

Shard è sistema di cache distribuito ma risolve il problema di localizzare cache con verità (cahce unica per entity)
No avanza a processare nuovo messaggio fino a quando sia cache che db non sono allineati.
read non va su db ma direttamente su cache.

Per quanto riguarda la parte **AP** si può prevedere l'utilizzo di **CRDT** (conflict free replicated data)
usano una replica e poi copiati in modo asincrono nelle altre repliche (con merge x es max(..) commutativa,associativa,idempotente)
(Akka Distribuited Data li fornisce) 
CRDT replicati su tutti i nodi e distribuiti quindi veloci da accedere.

come decidere fra Consistency o Availability? Business
