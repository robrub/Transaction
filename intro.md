# Transazioni

E' possibile immaginare una transazione come un contenitore per l'esecuzione di istruzioni SQL multiple (INSERT, UPDATE, SELECT, DELETE).

Tipicamente durante l'esecuzione di una tranzaizone il database potrebbe produrre (o lavorare) su dati inconsistenti perchè è possibile che durante l'esecuzione di più di un'istruzione SQL i dati del database potrebbero subire delle modifiche (dati inconsistenti).

Se una transazione viene eseguita con esito positivo significa che tutte le istruzioni SQL sono state oggetto di una COMMIT (Salvate).

In sistemi multi-utente con diverse operazioni che vengono eseguite nello stesso momento è importante garantire la consistenza dei dati durante queste transazioni e nel caso di problemi avere gli strumenti per attuare una strategia di "recovery".

## Le proprietà di una transazione (ACID)

ACID è l'acronimo di Atomicity, Consistency, Isolation e Durability.

- **(A)tomicity**: significa che tutte le operazioni che alterano i dati durante una transazioni devono essere gestite insieme. O tutte le operazioni vanno a buon fine (e quindi salvate) o in caso di errore tutte quante devono essere annullate.
- **(C)onsistency**: significa che una transazione deve sempre rispettare le regole di integrità del database. Quindi l'insieme delle operazioni della transazione devono essere conformi ai diversi constraint presenti nel database (primary key, foriegn key, data types, etc).
- **(I)solation**: è l'abilità di poter eseguire istruzioni SQL multiple senza che possano interferire con le altre in un ambiente "isolato". Questa caratteristica determina anche se i cambiamenti effettuati sui dati durante l'esecuzione di una transazione possono essere visibili alle altre transazioni.
- **(D)urability**: significa che una volta che una transazione è stata eseguita con esito positivo (commit) i dati devono essere salvati in modo permanente.
  
## I tipi di transazioni supportati da MySQL

### Read Uncommitted

In questo tipo di transazioni (che è il livello di isolamento più basso) le istruzioni della transazione possono leggere i dati "committati" dalle altre transazioni. Questo significa che altre transazioni in esecuzione possono alterare i dati che un'altra transazione sta leggendo.

### Read Committed

Questo è il secondo livello di isolamento. Questo tipo di transazione può leggere solo dati che sono già stati salvati (committed) da un'altra transazione.

### Repeatable Read (MySQL default)

Questo tipo di transazioni è un livello di isolamento superiore al precedente. Una transazione di questo tipo può leggere solo dati già salvati dalle altre transazioni (committed) e restringe la possibilità delle altre transazioni di alterare i dati che sta leggendo. Ciò vuol dire che se viene eseguita all'interno della transazione un'istruzione di SELECT più volte continuerà a restituire gli stessi dati.

### Serializable

Questo tipo di transazione è quella che ha il livello di isolamento più alto. In questo tipo di transazione sono disponibili in lettura solo i dati che sono stati oggetto di una commit da altre transazioni. Inoltre, se una transazione di questo tipo legge un dato, nessun'altra transazione può modificare quel dato fino a quando la transazione non viene completata.

## La tecnica di locking e la concorrenza delle transazioni di MySQL

Il meccanismo di locking è una tecnica che viene utilizzata per gestire la concorrenza delle transazioni. Questa tecnica viene utilizzata per evitare che più transazioni possano accedere allo stesso dato contemporaneamente.

MySQL gestisce quattro tipi di locking:

- **Shared locks**: più transazioni possono leggere lo stesso dato contemporaneamente ma vengono applicate delle restrizioni sulle operazioni di scrittura e modifica.
- **Exclusive locks**: questa tipologia di locking viene utilizzata per impedire che più transazioni possano accedere allo stesso dato contemporaneamente sia in lettura sia in scrittura.
- **Intent locks**: questo tipo di lock è usato per permettere a una transazione di specificare che leggerà o scriverà una certa porzione di dati.
- **Row-level locks**: permette a una transazione di effettuare il lock a livello di singola riga invece che fare un lock sull'intera tabella.

La concorrenza invece è un metodo applicato per permettere a più transazioni di essere eseguite simultaneamente senza interferenze tra una e l'altra. MySQL usa la "multi-version concurrency control" (MVCC) che permette a più transazioni di leggere e scrivere gli stessi dati nello stesso momento senza conflitti.

Ogni transazione acquisisce i dati che sta per modificare all'inizio della transazione e scrive le sue modifiche in una versione completamente diversa dei dati. Ciò consente ad altre transazioni di continuare a lavorare con la versione originale dei dati senza conflitti.

**Per implementare la concorrenza e non incorrere in conflitti è importante progettare e implementare transazioni più brevi possibili il cui tempo di esecuzione sia limitato nel tempo.**

[Preparazione dell'ambiente](/step1.md)
