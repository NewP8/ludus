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
Coordinator smista messaggi a Entità corretta
Bilanciamento delle entità diventa importante.

..Effects of Sharding
