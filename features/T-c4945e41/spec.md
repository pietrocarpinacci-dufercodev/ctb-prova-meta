# T-c4945e41 — Endpoint GET /api/todos/count

## Richiesta
Nel thread: "@ClaudeTeamsBot aggiungi all'API un endpoint GET /api/todos/count che restituisce il numero di todo, con un test".

## Criteri di accettazione
- WHEN un client invia GET /api/todos/count, l'API SHALL rispondere 200 con l'oggetto JSON `{ "count": N }`, dove N è il numero di todo presenti nello store.
- WHILE lo store è vuoto, l'API SHALL rispondere a GET /api/todos/count con `{ "count": 0 }`.
- WHEN viene creato un todo con POST /api/todos, GET /api/todos/count SHALL restituire `count` aumentato di 1.
- WHEN un todo viene segnato come fatto, GET /api/todos/count SHALL restituire lo stesso `count` (il conteggio include tutti i todo, fatti e non).
- WHEN un client invia GET /api/todos/{id} con id intero, l'API SHALL continuare a comportarsi come prima (nessuna collisione con `count`).

Fuori ambito: nessuna modifica al web (D2).
