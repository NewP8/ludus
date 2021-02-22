# CQRS


## Event sourcing

database state persistence - requisiti possono acmbiare
cambiamenti potrebbero riguardare stati del passato
ho stato attuale non storia di modifiche che mi ha portato a quello stato.

- spesso si affianca a db un **Audit Log** 
  (x riscostruire stato in caso di problemi o diagnosi)
- snapshot

Akka persistence e Lagom Persistent Entities

### Evoluzione del dominio

ES solo append (immutabile) - creare nuova versione per nuovo formto di evento ContoCreatedV2 x es

Command sourcing
comandi potrebbero essere eseguiti più volte


## Command Query Responsibility Segregation

separa Read Models (query) da Write Models (commands)

salvataggio aggregati
- se mi servono query che riguardano più aggregati (problema non solo di ES)

ES ho Event Store e Projection per Denormalized Store per query che mi servono
Akka persistence query e Lagom Read Side support

Read side che mi serve di più
(Si può creare Microservizio solo con dati che ci servono)

### Consistency, Availability e Scalability con CQRS

Flessibile su livello di consistenza
- write model strong
- read model eventual
sharding
  
in caso di fallimento sistema deve bloccare write side
Pros e cons...
-...stuf