# T-a9462771 — Priorità dei todo

## Richiesta
Nel thread Teams della feature T-a9462771 (link non disponibile nel job) il team ha chiesto di aggiungere una priorità ai todo:
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.

Nel thread il team ha accettato le proposte del piano per D1–D7: default `media`, ordine invariato, valori in italiano, `PUT .../priority`, filtro su un solo valore, nel web nessun filtro né modifica della priorità, colori proposti. Restano aperte D8 (chi lavora sul web) e D9 (maiuscole nei valori).

## Criteri di accettazione

### API
- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `bassa`, `media`, `alta` (D3).
- WHEN `POST /api/todos` riceve `priority` valida, l'API SHALL creare il todo con quella priorità.
- WHEN `POST /api/todos` non riceve `priority`, l'API SHALL creare il todo con priorità `media` (D1).
- IF `POST /api/todos` riceve una `priority` fuori dai tre valori THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority`.
- WHEN `PUT /api/todos/{id}/priority` riceve `{ "priority": "<valore valido>" }` per un todo esistente, l'API SHALL aggiornarne la priorità e restituire 200 con il todo aggiornato (D4).
- IF `PUT /api/todos/{id}/priority` riceve un valore non valido o mancante THEN l'API SHALL rispondere 400.
- IF `PUT /api/todos/{id}/priority` riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN `GET /api/todos?priority=<valore valido>` viene chiamato, l'API SHALL restituire solo i todo con quella priorità (D5).
- IF `GET /api/todos?priority=` riceve un valore non valido THEN l'API SHALL rispondere 400 (D5).
- WHEN `GET /api/todos` viene chiamato senza filtro, l'API SHALL restituire tutti i todo ordinati per id, come oggi (D2).
- Il confronto dei valori di priorità SHALL seguire la risposta a D9 (proposta: esatto, minuscolo).
- I comportamenti esistenti (titolo obbligatorio, 404 su id inesistente, `/done`, `/health`) SHALL restare invariati.
- `api/README.md` SHALL documentare il campo, l'endpoint di modifica e il filtro.

### Web
- La pagina SHALL mostrare accanto a ogni todo la sua priorità come testo, colorato in grigio `#6b7383` per bassa, ambra `#b54708` per media e rosso `#b42318` per alta (D7).
- Il form di creazione SHALL avere un menu (`<select>` con etichetta accessibile) con le tre priorità, preselezionato su `media` (D1).
- WHEN l'utente crea un todo, il web SHALL inviare all'API il titolo e la priorità scelta.
- IF la priorità passata a `addTodo` non è uno dei tre valori THEN `addTodo` SHALL rifiutare senza chiamare l'API.
- L'ordine dell'elenco nel web SHALL restare: prima i todo da fare, poi per id (D2).
- Il web SHALL NOT offrire il filtro per priorità né la modifica della priorità di un todo esistente (D6).
- `web/src/todos.js` e i test di entrambi i submodule SHALL riflettere il nuovo contratto (CLAUDE.md del meta-repo).
