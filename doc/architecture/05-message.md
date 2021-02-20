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





