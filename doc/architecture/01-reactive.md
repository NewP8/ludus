# Reactive Architecture

- chiuso per manutenzione
- aumento dei dati da elaborare/consumare
- aumento dei nodi
- utenti abituati a fidarsi (google, github, devops) - anche app utilizzano servizi cloud
- risposta immediata (sotto ogni condizione)

obiettivi
- scala 10-1000000 utenti
- cosuma solo risorse necessarie
- distribuito su più macchine
- mantiene livello di qualità

## Reactive Principles

Reactive Manifesto
unisce idee nate da esperienza di sviluppo software in 4 principi:
- responsive
   - confidenza
- resilient
  - replication: più copie
  - isolation: no dipendenza
  - containment: fallimento non si propaga
  - delegation: recovery delegata a componente esterna    
- elastic
- message driven
 - asincrono, non bloccante
 - permette disaccoppiamento

corrispondenza con ristorante (principi) - o altri software amazon,git...
 - vogliamo servire i clienti in meno di 10 min
 - se un cuoco si fa male devo poterlo sostituire
 - a natale ho bisogno di più personale
 - coda di foglietti con ordini, non attendo che piatto sia finito

Reactive system: architettura che rispetta principi reattivi (es. microservizi)
Reactive programming: Futures, Stream..

## Actor Model

paradigma di programmazione adatto a supportare lo sviluppo di sistemi reattivi.
In scala e JVM l'implementazione pricipale è **Akka**.
- tutte le computazioni gestite all'interno degli attori
- ogni attore ha un indirizzo (location transparency non so se locale o remoto)
- gli attori comunicano attraverso messaggi in modo asincrono
Senza attori
Location Transparency

