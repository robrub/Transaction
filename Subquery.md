# Subquery

Le subquery in SQL sono query annidate all'interno di un'altra query. Possono essere utilizzate per restituire dati che verranno utilizzati nella query principale come un valore temporaneo. Le subquery possono essere molto utili per eseguire operazioni complesse, come il filtraggio dei dati in base ai risultati di un'altra query.

## Esmpio 1

Subquery in una clausola WHERE: Questo tipo di subquery viene utilizzato per filtrare i risultati della query principale.

```sql
SELECT username, email
FROM users
WHERE service_type IN (SELECT service_type FROM services WHERE type = 'Premium');
```

In questo esempio, la subquery restituisce tutti i tipi di servizio che sono 'Premium'. Quindi, la query principale seleziona tutti gli utenti che hanno un tipo di servizio 'Premium'.

## Esempio 2

Subquery in una clausola FROM: Questo tipo di subquery viene utilizzato per creare una tabella temporanea che può essere utilizzata nella query principale.

```sql
SELECT avg(service_count)
FROM (
    SELECT service_type, count(*) as service_count
    FROM services
    GROUP BY service_type
) as service_summary;
```

In questo esempio, la subquery crea una tabella temporanea che raggruppa i servizi per tipo e conta quanti ce ne sono per ogni tipo. La query principale calcola poi la media di questi conteggi.

## Esempio 3

Subquery in una clausola SELECT: Questo tipo di subquery viene utilizzato per calcolare un valore che viene poi utilizzato nella selezione dei dati.

```sql
SELECT username, 
    (SELECT count(*) FROM orders WHERE orders.user_id = users.user_id) as order_count
FROM users;
```

In questo esempio, la subquery calcola il numero di ordini per ogni utente. Questo conteggio viene poi restituito come parte dei risultati della query principale.
