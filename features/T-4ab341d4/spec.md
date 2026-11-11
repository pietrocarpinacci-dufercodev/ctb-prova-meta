# T-4ab341d4 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams (feature T-4ab341d4) di aggiungere una priorità ai todo:
- API: campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore e un menu per sceglierla quando si crea un todo.

Il thread lascia esplicitamente aperti la priorità di default e l'ordine dell'elenco (vedi D1 e D2 in `plan.md`).

## Criteri di accettazione

### API
- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `{ "title": "...", "priority": "<valore valido>" }`, l'API SHALL salvarlo con quella priorità e rispondere 201 con il todo.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità di default (valore da D1).
- IF `priority` in creazione non è uno dei valori ammessi THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority` e SHALL NOT creare il todo.
- WHEN un client chiama l'endpoint dedicato di modifica (forma da D4) con una priorità valida su un todo esistente, l'API SHALL aggiornare la priorità e rispondere 200 con il todo aggiornato.
- IF l'endpoint di modifica riceve un id inesistente THEN l'API SHALL rispondere 404.
- IF l'endpoint di modifica riceve una priorità assente o non ammessa THEN l'API SHALL rispondere 400 e SHALL NOT modificare il todo.
- WHEN un client chiama `GET /api/todos?priority=<valore valido>`, l'API SHALL restituire solo i todo con quella priorità.
- WHEN un client chiama `GET /api/todos` senza filtro, l'API SHALL restituire tutti i todo come oggi.
- IF il filtro `priority` ha un valore non ammesso THEN l'API SHALL rispondere 400.
- L'ordine dell'elenco restituito dall'API SHALL seguire la risposta a D2 (fino ad allora resta l'ordine per id).
- Il README dell'API SHALL documentare il campo, l'endpoint di modifica e il filtro.

### Web
- WHILE l'elenco è mostrato, il web SHALL mostrare accanto a ogni todo la sua priorità come testo, con un colore diverso per `bassa`, `media`, `alta` (il colore non è l'unica indicazione).
- Il form di creazione SHALL contenere un menu (`<select>` con etichetta) con le tre priorità, preimpostato sul default di D1.
- WHEN l'utente crea un todo, il web SHALL inviare all'API la priorità scelta nel menu.
- L'ordine dell'elenco nel web SHALL seguire la risposta a D2.
- `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati al nuovo contratto (CLAUDE.md del meta-repo).
