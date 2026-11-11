# T-4ab341d4 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams (feature T-4ab341d4) di aggiungere una priorità ai todo:
- API: campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore e un menu per sceglierla quando si crea un todo.

Il thread lasciava aperti la priorità di default e l'ordine dell'elenco; poi ha accettato tutte le proposte del piano (D1–D7 in `plan.md` e `decisions.md`).

## Criteri di accettazione

### API
- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `{ "title": "...", "priority": "<valore valido>" }`, l'API SHALL salvarlo con quella priorità e rispondere 201 con il todo.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità `media` (D1).
- IF `priority` in creazione non è esattamente `bassa`, `media` o `alta` (minuscolo; per esempio `Alta` o `high`) THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority` e SHALL NOT creare il todo (D3).
- WHEN un client chiama `PUT /api/todos/{id}/priority` con `{ "priority": "<valore valido>" }` su un todo esistente, l'API SHALL aggiornare la priorità e rispondere 200 con il todo aggiornato (D4).
- IF `PUT /api/todos/{id}/priority` riceve un id inesistente THEN l'API SHALL rispondere 404.
- IF `PUT /api/todos/{id}/priority` riceve una priorità assente o non ammessa THEN l'API SHALL rispondere 400 e SHALL NOT modificare il todo.
- WHEN un client chiama `GET /api/todos?priority=<valore valido>`, l'API SHALL restituire solo i todo con quella priorità (un solo valore, D5).
- WHEN un client chiama `GET /api/todos` senza filtro, l'API SHALL restituire tutti i todo come oggi.
- IF il filtro `priority` ha un valore non ammesso THEN l'API SHALL rispondere 400.
- L'API SHALL continuare a restituire l'elenco ordinato per id (D2).
- Il README dell'API SHALL documentare il campo, il default, `PUT /api/todos/{id}/priority` e il filtro.

### Web
- WHILE l'elenco è mostrato, il web SHALL mostrare accanto a ogni todo la sua priorità come testo, colorato in verde per `bassa`, ambra per `media`, rosso per `alta` (il colore non è l'unica indicazione).
- Il form di creazione SHALL contenere un menu (`<select>` con etichetta) con le tre priorità, preselezionato su `media` (D1).
- WHEN l'utente crea un todo, il web SHALL inviare all'API la priorità scelta nel menu.
- Il web SHALL ordinare l'elenco mettendo prima i todo non fatti, poi per priorità alta → media → bassa, poi per id (D2).
- Il web SHALL NOT offrire in questa feature la modifica della priorità di un todo esistente né il filtro dell'elenco (D6).
- `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati al nuovo contratto (CLAUDE.md del meta-repo).
