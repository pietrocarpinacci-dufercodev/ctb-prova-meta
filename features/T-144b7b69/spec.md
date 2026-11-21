# T-144b7b69 — Priorità delle cose da fare

## Richiesta
Dal thread Teams della feature T-144b7b69 (il link al thread non è disponibile nel job):

> Vorrei aggiungere una priorità ai todo: nell'API un campo "priority" con valori bassa, media e alta, impostabile in creazione e modificabile con un endpoint dedicato, e la possibilità di filtrare l'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla quando si crea un todo. Non abbiamo deciso quale debba essere la priorità di default né l'ordine dell'elenco.

I riferimenti "(D*n*)" rimandano alle domande in `plan.md`, tutte risolte nel thread accettando le proposte: priorità di default `media`, valori JSON `bassa`/`media`/`alta`, endpoint `PUT /api/todos/{id}/priority`, 400 per un filtro non valido, ordine dell'elenco invariato. Il web non ha UI per cambiare la priorità o filtrare (D7). I testi della UI sono in italiano, i messaggi di validazione nuovi dell'API in inglese (D4).

## Criteri di accettazione

### API (`api`)
- Il sistema SHALL includere in ogni todo restituito dall'API il campo `priority`, con uno di questi valori: `bassa`, `media`, `alta` (D3).
- WHEN arriva `POST /api/todos` con un `priority` valido, il sistema SHALL creare il todo con quella priorità e rispondere 201.
- WHEN arriva `POST /api/todos` senza `priority`, il sistema SHALL creare il todo con la priorità di default `media` (D1).
- IF `POST /api/todos` contiene un `priority` fuori dai valori ammessi THEN il sistema SHALL rispondere 400 con un errore di validazione sul campo `priority`, senza creare il todo.
- WHEN arriva `PUT /api/todos/{id}/priority` con body `{ "priority": "<valore>" }` valido e il todo esiste, il sistema SHALL aggiornare la priorità e rispondere 200 con il todo aggiornato (D2).
- IF `PUT /api/todos/{id}/priority` riguarda un id inesistente THEN il sistema SHALL rispondere 404.
- IF `PUT /api/todos/{id}/priority` contiene un `priority` assente o non ammesso THEN il sistema SHALL rispondere 400 senza modificare il todo.
- WHEN si cambia la priorità, il sistema SHALL lasciare invariati `title` e `done`.
- WHEN arriva `GET /api/todos?priority=<valore>` con un valore ammesso, il sistema SHALL restituire solo i todo con quella priorità.
- WHEN arriva `GET /api/todos` senza `priority`, il sistema SHALL restituire tutti i todo, come oggi.
- IF `GET /api/todos?priority=<valore>` usa un valore non ammesso THEN il sistema SHALL rispondere 400 (D5).
- Il sistema SHALL restituire l'elenco nell'ordine attuale (per id), anche se filtrato (D6).

### Web (`web`)
- Il web SHALL mostrare la priorità accanto a ogni todo, con un'etichetta di testo e un colore diverso per ciascuno dei tre valori.
- Il web SHALL mostrare la priorità con il testo e non solo con il colore, perché sia leggibile anche da chi non distingue i colori.
- Il web SHALL mostrare nel form di creazione un menu (`<select>` con etichetta accessibile) con i tre valori, preselezionato sulla priorità di default (D1).
- WHEN l'utente crea un todo, il web SHALL inviare a `POST /api/todos` la priorità scelta nel menu.
- Il web SHALL ordinare l'elenco come oggi (prima i todo da fare, poi per id) (D6).

### Contratto e documentazione
- WHEN cambia il contratto, il sistema SHALL avere aggiornati `api/README.md`, l'elenco degli endpoint nel `CLAUDE.md` del meta-repo e in `.claude/agents/backend-dev.md`, `web/src/todos.js` e i test di entrambi i submodule.
- `dotnet test api` e `npm test --prefix web` SHALL passare.
