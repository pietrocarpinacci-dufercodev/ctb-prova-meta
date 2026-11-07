# T-e07d365f — Cancellazione dei todo

## Richiesta
Nel thread: aggiungi la cancellazione dei todo. Nell'API un endpoint `DELETE /api/todos/{id}` che risponde 204 se il todo esiste e 404 se non esiste, con i test. Nel frontend un pulsante "Elimina" accanto a ogni todo che chiama l'endpoint e aggiorna la lista.

## Criteri di accettazione
- WHEN arriva `DELETE /api/todos/{id}` con un id esistente, l'API SHALL rimuovere il todo e rispondere 204 senza corpo.
- IF l'id non esiste (o è già stato cancellato) THEN l'API SHALL rispondere 404.
- WHEN un todo è stato cancellato, `GET /api/todos/{id}` SHALL rispondere 404 e `GET /api/todos` non SHALL più includerlo.
- WHERE un todo è mostrato in lista (fatto o no), il frontend SHALL mostrare un pulsante "Elimina" accanto ad esso.
- WHEN l'utente preme "Elimina", il frontend SHALL chiedere conferma prima di procedere.
- IF l'utente annulla la conferma THEN il frontend SHALL non chiamare l'API e lasciare la lista invariata.
- WHEN l'utente conferma, il frontend SHALL chiamare `DELETE /api/todos/{id}` e poi ricaricare e ridisegnare la lista.
- IF la chiamata DELETE fallisce THEN il frontend SHALL mostrare l'errore nel box errori esistente, come per le altre azioni.
- IF la risposta è 204 THEN il frontend SHALL non leggere il corpo (nessun `response.json()`).
