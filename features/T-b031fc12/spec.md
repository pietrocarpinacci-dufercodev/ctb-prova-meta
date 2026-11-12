# T-b031fc12 — Priorità dei todo

## Richiesta
Nel thread Teams della feature T-b031fc12 il team ha chiesto di aggiungere una priorità ai todo:
- API: un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione, modificabile con un endpoint dedicato, e un filtro dell'elenco per priorità;
- web: la priorità accanto a ogni todo, con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.

Nel thread il team ha poi accettato tutte le proposte del piano (D1–D6 in `plan.md` e `decisions.md`): default `media`, ordine invariato, `PUT /api/todos/{id}/priority`, nel web niente filtro né modifica, colori grigio-blu/ambra/rosso, valori solo minuscoli.

## Criteri di accettazione
- Ogni todo restituito dall'API SHALL contenere il campo `priority`, una stringa tra `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `POST /api/todos` e `priority` valida, l'API SHALL salvare e restituire il todo con quella priorità (201).
- WHEN un client crea un todo con `POST /api/todos` senza `priority`, l'API SHALL assegnare la priorità `media` (D1).
- IF `POST /api/todos` riceve una `priority` diversa da `bassa`, `media`, `alta` (confronto esatto in minuscolo: `Alta` o `high` non sono validi, D6) THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority` e SHALL NOT creare il todo.
- WHEN un client chiama `PUT /api/todos/{id}/priority` con corpo `{ "priority": "<valore>" }` (D3) e una priorità valida, l'API SHALL aggiornare la priorità del todo e restituirlo (200), senza cambiare titolo e stato.
- IF l'endpoint dedicato riceve una priorità non valida o mancante THEN l'API SHALL rispondere 400.
- IF l'endpoint dedicato riceve l'id di un todo che non esiste THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con un valore valido, l'API SHALL restituire solo i todo con quella priorità.
- WHEN un client chiama `GET /api/todos` senza filtro, l'API SHALL restituire tutti i todo.
- IF `GET /api/todos` riceve un `priority` non valido THEN l'API SHALL rispondere 400.
- L'ordine dell'elenco dell'API e del web SHALL restare quello attuale: API per id; web prima i non fatti, poi per id (D2).
- Il web SHALL mostrare accanto a ogni todo la sua priorità come testo (`bassa`, `media`, `alta`) con un colore diverso per ciascun valore (bassa grigio-blu, media ambra, alta rosso, D5); il colore non è l'unica indicazione (accessibilità).
- Il modulo di creazione del web SHALL avere un menu (`<select>` con etichetta) con le tre priorità, preselezionato su `media` (D1).
- Il web SHALL NOT offrire in questa feature il filtro per priorità né la modifica della priorità di un todo esistente (D4).
- WHEN l'utente crea un todo dal web, il web SHALL inviare la priorità scelta nel corpo di `POST /api/todos`.
- `web/src/todos.js` SHALL validare la priorità prima di chiamare l'API e SHALL NOT chiamare l'API con un valore non valido.
- `api/README.md`, la sezione endpoint del `CLAUDE.md` del meta-repo e `.claude/agents/backend-dev.md` SHALL elencare il nuovo endpoint e il filtro.
- `dotnet test api` e `npm test --prefix web` SHALL passare, con test nuovi per creazione con e senza priorità, priorità non valida, endpoint dedicato (200/400/404), filtro (valido/non valido) e, nel web, invio della priorità e rifiuto di un valore non valido.
