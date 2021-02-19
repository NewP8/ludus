# Akka Cluster Sharding

Strumento per distribuire gli attori in un cluster
- per garantire *strong consistency*
- per ridurre accessi a DB

## Stateless systems

Le richieste devono contenere tutte le info necessarie(non fanno riferimento a stato)
- Utilizzano Db per recuperare lo stato e update
strong consistency garantita da Db 
  (che è anche single source of truth, single point of contention, songle point of failure)
  => è collo di bottiglia

- si può utilizzare Db sharding (meglio ma ancora bottleneck)
- oppure caching distribuito
  (problema con sistemi distribuiti mantenere consistenza di varie cache, no sigle point of truth)
  o centralizzato
  (Redis/Memcached ma lo stesso problema di garantire transazionalità)

## Akka cluster sharding 

permette sharding (non righe di db ma di attori).
Ogni attore ha Consistency coundary.

– attori (**Entity**) - unico id in cluster (single source of truth)
- gruppi di Entity in **Shard** (shard id ottenuto da entity id)
- Shards distrubuite in **Region**

Shard Coordinator (singleton usato per Distributed Data o Persistence per primo messaggio)
Importante che il numero delle entità nelle varie Shard sia bilanciato.

## Stateful (****)

State cachato fra i nodi sharded entity (attori distribuiti) – (Actor = Consistency Boundary e single source of truth)

Failure riguarda solo attori in nodo che fallisce (migrato in genere)- Bulkhead

Stateful Actor – cambia stato un messaggio alla volta e shard di attori un attore solo (read 1 volta n write uniche)

Passivation – non posso tenerle tutte in memoria (devo salvare stato da ricreare poi quando ricaricato)

Early/late

Rebalance- shard coordinator – solo se cluster è healthy  (altrimenti non sa se può procedere)

Messaggi in buffer

Remember entities per ricaricare auto dopo rebalance ma costoso

Numero minimo di membri x startup (restano joining e non possono creare shards) 