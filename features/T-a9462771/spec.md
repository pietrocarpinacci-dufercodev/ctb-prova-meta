# T-a9462771 — Priorità dei todo

## Richiesta
Nel thread Teams della feature T-a9462771 (link non disponibile nel job) il team ha chiesto di aggiungere una priorità ai todo:
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.

Non sono stati decisi la priorità di default né l'ordine dell'elenco (domande D1, D2 in `plan.md`). I criteri marcati "(proposta, Dn)" dipendono da una domanda aperta.

## Criteri di accettazione

### API
- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `bassa`, `media`, `alta` (proposta, D3).
- WHEN `POST /api/todos` riceve `priority` valida, l'API SHALL creare il todo con quella priorità.
- WHEN `POST /api/todos` non riceve `priority`, l'API SHALL creare il todo con la priorità di default `media` (proposta, D1).
- IF `POST /api/todos` riceve una `priority` fuori dai tre valori THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority`.
- WHEN `PUT /api/todos/{id}/priority` riceve `{ "priority": "<valore valido>" }` per un todo esistente, l'API SHALL aggiornarne la priorità e restituire 200 con il todo aggiornato (proposta, D4).
- IF l'endpoint di modifica priorità riceve un valore non valido o mancante THEN l'API SHALL rispondere 400.
- IF l'endpoint di modifica priorità riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN `GET /api/todos?priority=<valore valido>` viene chiamato, l'API SHALL restituire solo i todo con quella priorità (proposta, D5).
- IF `GET /api/todos?priority=` riceve un valore non valido THEN l'API SHALL rispondere 400 (proposta, D5).
- WHEN `GET /api/todos` viene chiamato senza filtro, l'API SHALL restituire tutti i todo nell'ordine attuale per id (proposta, D2).
- I comportamenti esistenti (titolo obbligatorio, 404 su id inesistente, `/done`, `/health`) SHALL restare invariati.
- `api/README.md` SHALL documentare il campo, l'endpoint di modifica e il filtro.

### Web
- La pagina SHALL mostrare accanto a ogni todo la sua priorità come testo, con un colore diverso per ciascuno dei tre valori (proposta colori in D7).
- Il form di creazione SHALL avere un menu (`<select>` con etichetta accessibile) con le tre priorità, preselezionato sul default (proposta, D1).
- WHEN l'utente crea un todo, il web SHALL inviare all'API il titolo e la priorità scelta.
- IF la priorità passata a `addTodo` non è uno dei tre valori THEN `addTodo` SHALL rifiutare senza chiamare l'API.
- L'ordine dell'elenco nel web SHALL restare: prima i todo da fare, poi per id (proposta, D2).
- `web/src/todos.js` e i test di entrambi i submodule SHALL riflettere il nuovo contratto (CLAUDE.md del meta-repo).
