# Akka cluster

Permette la distribuzione degli Attori Akka fra i nodi di un cluster.
In questo modo si riesce a bilanciare i carichi di lavoro e accessi a DB.
Scalabilità orizzontale al posto di scalabilità verticale.

Sistemi tradizionali ogni nodo non conosce gli altri mentre con Akka Cluster è il contrario.

Akka Cluster Sharding distribuisce attori
- distribuisce carichi di lavoro (Akka Cluster Aware Routers)
- permette di ridurre accesso a DB (Akka Cluster Sharding)
- evita problematiche legate a sistemi di cache per stato condiviso (Akka DistributedData)

Ogni attore mantiene il proprio stato (non serve leggere ogni volta il db)
L'Actor model garantisce consistenza fra stato e db.

## Formazione di un Cluster

Ogni attore ha indirizzo, protocollo di trasporto (`akka.remote.artery`), uuid (identifica istanza nel tempo/riavvi)
Akka Cluster sfrutta le funzionalità di Akka remote e permette di raggruppare in cluster Attori distribuiti su più nodi.
Attori appartenenti a nodi diversi ma appartenenti ad ActorSystems **con lo stesso** nome vengono raggruppati in cluster.
(configurazione `akka.actor.provider = "cluster"`).
Ogni istanza (hostname, porta, uuid) può unirsi al cluster **solo una volta**

### joining

Per unirsi a cluster un nodo deve contattare dei **nodi seed** (usati per la formazione del cluster, poi nodi normali).
Si possono configurare staticamente `akka.cluster.seed-nodes = [..]` o utilizzare Akka Cluster Bootstrap che determina
dinamicamente quali saranno i seed-nodes.

Il primo nodo seed inizia provando join
- se c'è cluster -> join
- se non c'è -> forma un cluster da se

gli altri nodi non seed hanno bisogno che un cluster sia gia formato
(esempio partizione: 5 nodi 3 seed nodes. se i tre seed si riavviano i 2 rimangono in cluster separato)

Il cluster così formato può essere utilizzato ad esempio da Cluster Sharding per mantenere uno stato condiviso.
(esempio balance)

- HTTP CLuster Management: permette di visualizzare via http lo stato del cluster (8558)...

## Gossip e ciclo di vita dei nodi

Ogni nodo manda la propria visione del cluster ad un altro nodo random (versione):
- (nodi, stato, visto)

quando tutti hanno visto -> **convergenza** (comunque gossip continua sempre) -> Nodo con id più basso diventa **Leader**

Joining -> Up -> Leaving -> Exiting -> Removed (devefare restart per rejoin)

### Failure

disconnesso da cluster viene marcato come **Unreachable** (può tornare Reachable non cambia stato)
In alcuni casi si può voler marcare attore com **Down** in modo da cambiare stato a Removed permettendo restart.

Ogni nodo è monitorato da 5 altri che guardano storia di heartbeat.
Se un nodo rileva failure di altro nod inizia gossip dicendo che è Unreachable 
tutti continuano a fare health-check.

le cause di non raggiungibilità non sempre legate a problemi di rete ma spesso a problemi dell' applicazione
(meglio indagarle prima di settare un meccanismo di failure detection!).

Un volta che un nodo è Unreachable la convergenza diventa impossibile!
Non si può sceglere leader - nessuna azione di leader può essere fatta
nessun nodo può unirsi a cluster (deve tornare reachable o Down)
Per i membri nuovi fino a quando non si sistema no passano ad Up ma a WeaklyUp (poi con convergenza Up)


#### Healing

Se nodo va in stato di fallimento -> Facendo ripartire nodo riconosce che è nuova istanza con stesso nome e fa rejoin veloce.
Se si verifica partizione di rete -> devo decidere quale parte della rete tenere, terminare attore e informare cluster che sono Down, infine creare i sostituti.
(`PUT .. -F operation=down <indirizzo-akka-management>/cluster/members/<nome-attore>`)

problema => **Split Brain**
es se metto a Down nodo che non è terminato.

il problema è cosa accade se ho stato
Cluster Sharding e Singleton usati per condividere stato in cluster
se plitbrain si formano più cluster e non posso garantire che shard è univoca ad esempio.
(overwrite o corruzione dei dati- in ES eventi sovrapposti)

Con intervento a mano si può evitare splitbrain ma SBR può gestire in modo automatico.
(Auto-Downing è pericoloso! - tolto da 2.6)

Con **Split Brain Resolver** fornisce strategie per terminazionde di attori evitando SB.
- static quorum
- keep-majority
- keep-oldest
- keep-referee
- down-all
- lease-majority

diversi casi:











