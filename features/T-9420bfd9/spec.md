# T-9420bfd9 — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-9420bfd9 (nel job non c'era il link al thread): aggiungere una priorità ai todo.
- API: un campo `priority` con valori `bassa`, `media` e `alta`, da impostare quando si crea un todo, da modificare con un endpoint dedicato, e un filtro per priorità sull'elenco.
- Web: la priorità accanto a ogni todo, con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.
- Il thread non ha ancora deciso la priorità di default né l'ordine dell'elenco (vedi D1 e D2 in `plan.md`).

## Criteri di accettazione
Valori proposti nel piano, ancora da confermare: formato JSON (D3), endpoint (D4), filtro non valido (D5). Il default `<default D1>` e l'ordine `<ordine D2>` restano da decidere.

API (`api`)
- Il sistema SHALL restituire in ogni todo di `GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done` il campo `priority`, con valore `"bassa"`, `"media"` o `"alta"`.
- WHEN si chiama `POST /api/todos` con `priority` valida, il sistema SHALL creare il todo con quella priorità.
- WHEN si chiama `POST /api/todos` senza `priority` (assente o `null`), il sistema SHALL creare il todo con la priorità `<default D1>`.
- IF `POST /api/todos` riceve una `priority` diversa da `bassa`, `media` e `alta` THEN il sistema SHALL rispondere 400 con un errore di validazione sul campo `priority` e SHALL non creare il todo.
- WHEN si chiama `PUT /api/todos/{id}/priority` con body `{ "priority": "<valore valido>" }` su un todo esistente, il sistema SHALL aggiornare la priorità e rispondere 200 con il todo aggiornato, lasciando invariati titolo e stato.
- IF `PUT /api/todos/{id}/priority` riceve una priorità assente o non valida THEN il sistema SHALL rispondere 400 e SHALL lasciare il todo invariato.
- IF `PUT /api/todos/{id}/priority` riguarda un id inesistente THEN il sistema SHALL rispondere 404.
- WHEN si chiama `GET /api/todos?priority=<valore valido>`, il sistema SHALL restituire solo i todo con quella priorità.
- WHEN si chiama `GET /api/todos` senza il parametro `priority`, il sistema SHALL restituire tutti i todo, come oggi.
- IF `GET /api/todos` riceve un parametro `priority` non valido THEN il sistema SHALL rispondere 400.
- Il sistema SHALL restituire l'elenco nell'ordine `<ordine D2>`.

Web (`web`)
- Il modulo di creazione SHALL avere un menu (`<select>` con etichetta) con le opzioni bassa, media e alta, preselezionato su `<default D1>`.
- WHEN l'utente aggiunge un todo, il web SHALL mandare all'API la priorità scelta nel menu.
- Il web SHALL mostrare accanto al titolo di ogni todo la priorità come testo, con un colore diverso per ciascuno dei tre valori.
- IF un todo arriva dall'API senza `priority` o con un valore sconosciuto THEN il web SHALL mostrarlo senza errori e senza colore di priorità.
- Il web SHALL ordinare l'elenco secondo `<ordine D2>`, in coerenza con l'API.

Contratto e documentazione
- WHEN cambia il contratto, `api/README.md`, l'elenco degli endpoint in `CLAUDE.md` del meta-repo, `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati nella stessa feature.
