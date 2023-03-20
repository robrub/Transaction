# Transazioni

In un database *SQL (Structured Query Language)*, le transazioni sono un meccanismo per garantire l'integrità dei dati durante le operazioni di aggiornamento. Una transazione rappresenta un insieme di operazioni di database che devono essere eseguite in modo atomico, cioè come se fossero una singola operazione, e deve essere eseguita nella sua interezza o annullata in caso di errori.

## Le diverse tipologie di transazione

Esistono quattro tipologie di transazioni supportate dai database SQL:

- **Read uncommitted**: questa tipologia di transazione può essere utilizzata in situazioni in cui l'integrità dei dati non è particolarmente critica e si desidera ottenere le prestazioni più elevate possibili. Ad esempio, potrebbe essere utilizzata in un'applicazione web in cui si leggono i dati in tempo reale, ma l'aggiornamento dei dati non deve essere eseguito in modo atomico.

- **Read committed**: questa tipologia di transazione è adatta per le situazioni in cui la coerenza dei dati è importante, ma non è necessario garantire la ripetibilità della transazione. Ad esempio, potrebbe essere utilizzata in un'applicazione di gestione dei clienti in cui i dati dei clienti devono essere aggiornati rapidamente, ma non è necessario garantire che una transazione di lettura venga eseguita sempre con gli stessi dati.
  
- **Repeatable read**: questa tipologia di transazione è adatta per le situazioni in cui la ripetibilità della transazione è importante e non è possibile utilizzare una transazione di livello superiore. Ad esempio, potrebbe essere utilizzata in un'applicazione di contabilità in cui i dati finanziari devono essere sempre coerenti e riproducibili, anche se altri utenti eseguono operazioni di aggiornamento.

- **Serializable**: questa tipologia di transazione è adatta per le situazioni in cui l'integrità dei dati è particolarmente critica e deve essere garantita in modo rigoroso. Ad esempio, potrebbe essere utilizzata in un'applicazione di gestione degli ordini in cui i dati degli ordini devono essere sempre coerenti e non è possibile accettare sovrapposizioni tra le operazioni di aggiornamento e di lettura.

È importante scegliere la tipologia di transazione giusta in base alle esigenze dell'applicazione, in modo da garantire l'integrità dei dati e una buona performance delle operazioni di database.

## Read uncommitted

``` SQL
BEGIN TRANSACTION;
SELECT * FROM mytable WITH (NOLOCK);
COMMIT TRANSACTION;
```

In questo esempio, la transazione legge i dati dalla tabella "mytable" senza acquisire il blocco dei dati, il che significa che i dati non sono ancora confermati e potrebbero essere in fase di modifica da altre transazioni.

## Read committed

``` SQL
BEGIN TRANSACTION;
SELECT * FROM mytable;
UPDATE mytable SET column1 = 'newvalue' WHERE column2 = 'somevalue';
COMMIT TRANSACTION;
```

In questo esempio, la transazione legge i dati dalla tabella "mytable" e successivamente aggiorna i dati in base a una determinata condizione. La transazione garantisce che i dati letti siano confermati e quindi non in fase di modifica da altre transazioni.

## Repeatable read

``` SQL
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRANSACTION;
SELECT * FROM mytable WHERE column1 = 'somevalue';
UPDATE mytable SET column2 = 'newvalue' WHERE column1 = 'somevalue';
COMMIT TRANSACTION;
```

In questo esempio, la transazione legge i dati dalla tabella "mytable" in base a una determinata condizione e successivamente aggiorna i dati in base alla stessa condizione. La transazione garantisce che i dati letti durante la transazione rimangano gli stessi, anche se altri utenti eseguono operazioni di aggiornamento sui dati letti.

## Serializable

``` SQL
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN TRANSACTION;
SELECT * FROM mytable WHERE column1 = 'somevalue';
UPDATE mytable SET column2 = 'newvalue' WHERE column1 = 'somevalue';
COMMIT TRANSACTION;
```

In questo esempio, la transazione legge i dati dalla tabella "mytable" in base a una determinata condizione e successivamente aggiorna i dati in base alla stessa condizione. La transazione garantisce l'isolamento completo delle transazioni, garantendo che non ci sia sovrapposizione tra le operazioni di aggiornamento e di lettura.