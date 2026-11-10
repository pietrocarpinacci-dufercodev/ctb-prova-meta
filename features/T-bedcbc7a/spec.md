# T-bedcbc7a — API più completa

## Richiesta
Il team ha chiesto nel thread Teams: «sviluppami una api complessissima che sia in grado di fare tutto». La richiesta non ha un perimetro: la spec copre solo il minimo ragionevole (completare il CRUD sulle cose da fare) finché le domande aperte in `plan.md` non sono risolte.

## Criteri di accettazione
- WHEN arriva `PUT /api/todos/{id}` con `{ "title": "..." }` valido, l'API SHALL aggiornare il titolo e rispondere 200 con la cosa da fare aggiornata.
- IF il titolo di `PUT /api/todos/{id}` è vuoto THEN l'API SHALL rispondere 400.
- IF l'id di `PUT` o `DELETE /api/todos/{id}` non esiste THEN l'API SHALL rispondere 404.
- WHEN arriva `DELETE /api/todos/{id}` per una cosa da fare esistente, l'API SHALL rimuoverla dall'elenco in memoria e rispondere 204.
- WHEN arriva `GET /api/todos?done=true|false`, l'API SHALL restituire solo le cose da fare con quello stato.
- WHILE gli endpoint esistenti (`/api/health`, `/api/todos`, `/api/todos/{id}`, `POST /api/todos`, `/api/todos/{id}/done`) non cambiano contratto, l'API SHALL continuare a rispondere come descritto in `api/README.md`.
- WHERE un endpoint nuovo è usato dalla pagina web, `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati nello stesso cambio.
