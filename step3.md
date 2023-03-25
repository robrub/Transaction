# Gestire errori ed eccezioni all'interno di una transazione

Gestire gli errori e le eccezioni è importante in qualsiasi applicaivo. In MySQL, quando si esegue una transazione, è importante gestire gli errori e le eccezioni per evitare che la transazione venga eseguita in modo parziale o anche solo per avvertire l'utente.

Ad esempio in Java o Javascript per gestire un eccezzione di solito si usa un blocco `try-catch`:

In MySQL SIGNAL e RESIGNAL sono le istruzioni che permettono di gestire gli errori e le eccezioni.

Vediamo un esempio:

```sql
-- Inizio transazione
START TRANSACTION;

    -- Definiamo un blocco per gestire il "catch" dell'eccezione
    -- che si potrebbe verificare.
    -- Se si verifica un errore viene chiamato questo blocco 
    -- `DECLARE EXIT HANDLER FOR SQLEXCEPTION` e vengono eseguite le istruzioni al suo interno (un ROLLBACK).
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
        BEGIN;
            ROLLBACK;
            RESIGNAL; -- Resstituisce l'eccezione al chiamante (in questo caso il client MySQL)
        END;

        -- Inserimento di un nuovo utente che non esiste nella tabella
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname3','user3@gmail.com','F');

    -- Inserimento di un utente che già esiste nella tabella (chiave duplicata)
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname2','user2@gmail.com','F');
    
COMMIT;
```

Possiamo anche inserire della logica applicativa per gestire le eccezioni che non riguardano strettamente le regole di integrità dei dati ma anche regole di business.

Prendiamo il seguente script dove in una nondizione particolare vogliamo che la transazione venga annullata e scatenare un'eccezione:

```sql
-- Inizio transazione
START TRANSACTION;

    -- Definiamo un blocco per gestire il "catch" dell'eccezione
    -- che si potrebbe verificare.
    -- Se si verifica un errore viene chiamato questo blocco 
    -- `DECLARE EXIT HANDLER FOR SQLEXCEPTION` e vengono eseguite le istruzioni al suo interno (un ROLLBACK).
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
         BEGIN;
            ROLLBACK;
            RESIGNAL; -- Resstituisce l'eccezione al chiamante (in questo caso il client MySQL)
        END;

    -- Inserimento di un nuovo utente che non esiste nella tabella
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname4','user4@gmail.com','F');

    -- Inserimento di un utente che già esiste nella tabella (chiave duplicata)
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname4','user4@gmail.com','F');

    -- Dopo l'inserimento dei due utenti nuovi (le istruzioni
    -- non daranno errore) vogliamo fare un controllo su una
    -- particolare condizione e se questa condizione non è vera 
    -- scatenare noi un'eccezione e di conseguenza annullare la transazione.
    IF (SELECT count(*) FROM collections ) > 100 THEN
        -- Questa istruzione scatena un'eccezione che verrà 
        -- gestita dal blocco `DECLARE EXIT HANDLER FOR SQLEXCEPTION`
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Eccezione custom';
    END IF;
    
COMMIT;
```
