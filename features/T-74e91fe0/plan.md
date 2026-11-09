# T-74e91fe0 — Piano

## Submodule coinvolti
- api: campo `priority` nel record `Todo`/`NewTodo` e in `TodoStore`, validazione, endpoint dedicato per cambiarla, filtro `?priority=` su `GET /api/todos`, test in `TodoApiTests.cs`, tabella endpoint in `README.md`.
- web: `todos.js` (invio `priority` in `addTodo`, ordinamento; nessuna modifica della priorità né filtro nel web, D3/D4), `index.html` (menu di scelta), `app.js` (render priorità accanto al titolo), `style.css` (un colore per valore), test in `tests/todos.test.js`.

## Passi
1. API: definire i valori ammessi (`bassa`, `media`, `alta`), default `media` (D1); aggiungere `Priority` al record `Todo` e a `NewTodo` (opzionale), serializzata come stringa minuscola (D5), accettata senza distinzione di maiuscole.
2. API: validare la priorità in `POST /api/todos` (400 se non valida) e applicare il default se assente.
3. API: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` (D3), 404 se il todo non esiste, 400 se il valore non è valido.
4. API: filtro `?priority=` su `GET /api/todos`, 400 se il valore è sconosciuto (D4); l'API mantiene l'ordine per id (D2).
5. API: test (xUnit, `dotnet test api`) per creazione con/senza priorità, priorità non valida, modifica, filtro, 404; aggiornare `api/README.md`.
6. Web: `todos.js` invia `priority` in creazione, gestisce l'assenza del campo con il default, ordina in `sortTodos` per non fatti prima, poi priorità alta→bassa, poi id (D2); test con `npm test --prefix web`.
7. Web: menu `<select>` nel form, badge priorità accanto al titolo con classe CSS per valore, tre colori in `style.css`.
8. Verifica incrociata del contratto (CLAUDE.md meta-repo): nomi dei valori e del campo identici in API e web.

## Fonti
- `CLAUDE.md` del meta-repo (contratto API↔web, comandi di test).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`.
- `web/src/todos.js`, `app.js`, `index.html`, `style.css`, `web/tests/todos.test.js`.
- `features/_template/*`; nessuna altra feature con `decisions.md`; nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: modifica piccola, in memoria, nessuna infrastruttura.
- Complessità: bassa; un campo, un endpoint, un filtro, un menu.
- Sicurezza: validare i valori in ingresso (creazione, modifica, filtro); nessun dato sensibile.
- Accoppiamento: il contratto cambia in API e web insieme, quindi due PR da tenere allineate; il campo è retrocompatibile (opzionale in creazione).

## Domande aperte
- [x] D1: Priorità di default — risposta: `media` (proposta accettata nel thread), anche per i todo senza campo lato web.
- [x] D2: Ordine dell'elenco — risposta: proposta accettata; il web ordina non fatti prima, poi priorità alta→bassa, poi id. L'API resta ordinata per id (scelta del piano per l'ambiguità "solo web o anche API", minimo cambiamento).
- [x] D3: Endpoint dedicato — risposta: `PUT /api/todos/{id}/priority` con `{ "priority": "alta" }`. Il web sceglie la priorità solo alla creazione, come da richiesta; nessuna modifica su todo esistenti.
- [x] D4: Filtro sconosciuto — risposta: 400. Il filtro è solo nell'API, non nel web.
- [x] D5: Valori JSON — risposta: stringhe `bassa`/`media`/`alta`, accettate senza distinzione di maiuscole in ingresso e restituite in minuscolo.
