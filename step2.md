# Come gestire una transazione in MySQL

## Creare una transazione

Per poter creare una transazione in MySQL, è necessario utilizzare il comando `START TRANSACTION`:

```sql

/* inizia la transazione (indico a MySQL che voglio fare una 
   transazione e tutte le istruzioni succesive ne fanno parte) */
START TRANSACTION; 
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname1','user1@gmail.com','F');
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname2','user2@gmail.com','F');
```

In questo esempio abbiamo creato una transazione che contiene due istruzioni `INSERT`. Se entrambe le istruzioni vanno a buon fine, la transazione viene eseguita correttamente. Se invece una delle due istruzioni fallisce, la transazione non viene eseguita e viene annullata (rollback).

**ATTENZIONE:** in questo esempio la transazione non ha una chiusura (COMMIT o ROLLBACK). Solo con un'istruzione che indica la fine della transazione, MySQL esegue la transazione. Se non viene specificata una chiusura, MySQL esegue la transazione in automatico alla fine della connessione.

L'istruzione completa è la seguente:

```sql
START TRANSACTION; 
    
    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname1','user1@gmail.com','F');

    INSERT INTO users(username, email, service_type) 
    VALUES ('mynickname2','user2@gmail.com','F');

COMMIT;
```

Solo nel momento in chi viene eseguita l'istrzuione `COMMIT`, la transazione viene "salvata" e i dati saranno inseriti in modo permanente nella tabella. Se una delle due istruzioni fallisce, la transazione non viene eseguita e salvata ma viene annullata (rollback).

## Un altro esempio

Dopo aver eseguito le istruzioni precedenti (inserimento di due utenti), proviamo ad eseguire un'altra transazione.

Prima controlliamo il contenuto della tabella `users`:

```sql
SELECT * FROM users;
```

Verranno elencati i due utenti inseriti nella transazione precedente.
Ora proviamo ad eseguire una transazione che elimina un utente senza `COMMIT`:

```sql
START TRANSACTION; 
    DELETE FROM users WHERE username = 'mynickname1';
```

L'output dell'esecuzione ci indicherà che una riga nella tabella è stata eliminata.

Proviamo ad **aprie una nuova connessione MySQL** e a controllare il contenuto della tabella `users`:

```sql
SELECT * FROM users;
```

Anche se nella preceente connessione noi abbiamo eliminato l'utente in questa seconda connessione continuiamo a vederlo all'interno della tabella. Questo perché la transazione non è stata eseguita e salvata con un `COMMIT` o annullata con un `ROLLBACK`.

[Gestione degli errori](step3.md)
