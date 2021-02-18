# Domain Driven Design

origine - libro blu di Eric Evans
suddividere domini grandi in pezzi più piccoli individuando limiti e responsabilità 
DDD e architetture reattive (microservizi) vanno di pari passo (molto ben compatibili)

## Domain

Un dominio è una sfera di conoscenza (area di conoscenza, idea compito da modellare) 
Esperti di dominio e sviluppatori
obiettivo è creare un modello che poi permetta implementazione software.
Ubiquitous Language per comunicare

### Scomporre il dominio

Grandi domini = complessità (interazioni complesse)

 - subdomain (possibili sovrapposizioni) 
   ogniuno con suo modello + UL (= Bounded Context)
   
il significato delle parole puù cambiare in base a con chi si parla
Se ha 2 significati e info importanti diverse probabilmente appartiene a due BC diversi (x es id oggetti in ordine utile a magazzino no a destinatario)
descrivere non solo oggetti ma anche eventi e attività (event storming)

### Attività

 - notazione **soggetto-verbo-oggetto** (x attività) - oggetti diretti e indiretti 
 - Mantere la purezza dei BC; evitare di mischiare piuttosto utilizzare **Anti-Corruption Layers** (interfaccia x traduzone fra concetti).
 - Context Map per tracciare dipendenza e orrelazioni fra BC
 - comandi (richieste cambio stato) vs eventi (accaduto) vs query (quali info voglio) sono messaggi compresi in API di BC

### Oggetti

 - Value Objects: definiti dagli attributi (immutabile)
 - Entities: definiti da ID (mutabile con business logic) - Actors
 - Aggregates collezione di Entity con entità root (accesso a entità tramite root)
   .. utile pensare a cosa comporta cancellazione.. VO possono essere passati fra BC mentre Aggregati è smell di problema
     
### Services Factories e Repositories

Services: stateless per business logic non catturabili in oggetti (x es in ACL)
Factories: x creazione da sistemi esterni
Repositories: x update da sistemi esterni 

## Hexagonal Architecture (Ports and Adapters)

Spesso in relazione con DDD (anche se concetti separati)
Visione non a livelli (UI-dom-db) ma Dominio centrale espone porte(api) e adapters(infrastructure) comunicano attraverso queste porte.
user side / data side.
Domain - API - Infrastructure (<- dipendenza e separazione)
In lagom Components
