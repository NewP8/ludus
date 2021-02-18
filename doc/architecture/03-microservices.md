# Microservices

Monolith <---> Microservices

## Monolith

 - ball of Mud (no isolamento e dipendenze complesse) - si può separare concetti in librerie o package
 - single shared DB (garantisce consistenza fra repliche)
 - comunicazione sincrona
 - "big bang" release
    
pro) cross module refactoring semplice, semplice mantenere consistenza, semplice deploy, semplice da monitorare e modello scalabilità semplice
cons) limite risorse macchine, database è limite per scalabilità, accoppiamento rende difficile modifiche, failure possono espandersi a cascata (carico redistribuito)

## SOA - Service Oriented Architecture

Creo isolamento suddividendo i vari servizi ogniuno con proprio database e API.
Deployabili insieme o in modo indipendente
Microservices sono sottoinsieme di SOA

## Microservices

- possono essere deployati in modo indipendente.
- db indipendenti
- comunicazione sincrona o asincrona
- scarso accoppiamento
- sviluppo rapido (continuo)
- ciclo di release flessibile
- organizzazione team spesso **DevOps**

pro: scalabilità, disponibilità (isolamento fallimenti), disaccoppiamento, supporta diverse piattaforme/linguaggi
cons: monitor e deploy + complesso, refactoring cross reference, supporto vecchie API,  cambi organizzativi

### Single Responsibility

Cambiamento non deve influenzare altri microservizi (utile utilizzare Bounded context per individuare limiti)
Scomposizione del Monolite -> azione quali microservizi coinvolge e quali chiamate deve fare

### Isolamento

- state: accesso tramite API
- space: posso spostare datacenter e scalare
- time: richieste asincrone (-> eventual consistency fra ms)
- failure: fail di ms non deve comportare fail di altro microservizio

- bulkheading
- Circuit Breakers: evitare sovraccarico fallendo prima
- Message driven: mesaggi asincroni permettono disaccoppiamento tempo e fallimenti (se chiedo aiuto e stò a guardare)
- autonomia (ha info per risolvere conflitti, con internal local state x es)

### API Gateway Services

Complessità delle API 
- chiedo info sparsa in diversi microservizi
- è microservizio che conosce e coordina comunicazione con gli altri (aggrega info eventualmente con cache etc..)



