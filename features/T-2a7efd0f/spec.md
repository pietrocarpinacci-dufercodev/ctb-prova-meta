# T-2a7efd0f — Endpoint GET /api/todos/count

## Richiesta
Il team ha chiesto nel thread Teams: "Aggiungi un endpoint GET /api/todos/count che restituisce il numero dei todo".

## Criteri di accettazione
- WHEN arriva GET /api/todos/count, l'API SHALL rispondere 200 con il numero totale dei todo presenti in memoria (formato del corpo: vedi D1 in plan.md; default previsto `{ "count": n }`).
- WHILE non esiste alcun todo, l'API SHALL rispondere a GET /api/todos/count con conteggio 0.
- WHEN viene creato un todo con POST /api/todos, GET /api/todos/count SHALL restituire un conteggio incrementato di 1.
- WHEN arriva GET /api/todos/{id} con id intero, l'API SHALL continuare a comportarsi come prima (la rotta `count` non deve collidere con `{id:int}`).
- WHERE l'endpoint è aggiunto, `api/README.md` SHALL documentarlo nella tabella degli endpoint.
