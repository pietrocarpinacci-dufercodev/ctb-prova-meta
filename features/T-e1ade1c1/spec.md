# T-e1ade1c1 — Endpoint DELETE /api/todos/completed

## Richiesta
Il team ha chiesto nel thread Teams: «Aggiungi un endpoint DELETE /api/todos/completed che elimina i todo completati.»

## Criteri di accettazione
- WHEN arriva `DELETE /api/todos/completed`, l'API SHALL eliminare dallo store tutti i todo con `Done = true`.
- WHEN la richiesta ha successo, l'API SHALL lasciare invariati i todo non completati.
- WHEN la richiesta ha successo, l'API SHALL rispondere 204 No Content (vedi D1 nel piano).
- IF non esistono todo completati THEN l'API SHALL rispondere comunque 204 senza modificare lo store.
- WHEN la richiesta ha successo, `GET /api/todos` SHALL non restituire più i todo eliminati e `GET /api/todos/{id}` SHALL rispondere 404 per i loro id.
- WHERE `api/README.md` documenta gli endpoint, esso SHALL elencare il nuovo endpoint.
