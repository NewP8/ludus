# Akka

## Akka cluster

## Akka Cluster Sharding

Stateless

Problemi Db

Sharding + cache



Akka cluster sharding – distribuzone degli attori (entity)

Entity distribuite in shard (shard id da entity id)

Region contiene shards

Coordinator (Distributed Data o Persistence)

Shard in region, region in nodo



Stateful

State cachato fra i nodi sharded entity (attori distribuiti) – (Actor = Consistency Boundary e single source of truth)

Failure riguarda solo attori in nodo che fallisce (migrato in genere)- Bulkhead

Stateful Actor – cambia stato un messaggio alla volta e shard di attori un attore solo (read 1 volta n write uniche)

Passivation – non posso tenerle tutte in memoria (devo salvare stato da ricreare poi quando ricaricato)

Early/late

Rebalance- shard coordinator – solo se cluster è healthy  (altrimenti non sa se può procedere)

Messaggi in buffer

Remember entities per ricaricare auto dopo rebalance ma costoso

Numero minimo di membri x startup (restano joining e non possono creare shards) 