# T-74e91fe0 — Piano

## Submodule coinvolti
- api: campo `priority` nel record `Todo`/`NewTodo` e in `TodoStore`, validazione, endpoint dedicato per cambiarla, filtro `?priority=` su `GET /api/todos`, test in `TodoApiTests.cs`, tabella endpoint in `README.md`.
- web: `todos.js` (invio `priority` in `addTodo`, funzione per cambiarla se serve, ordinamento), `index.html` (menu di scelta), `app.js` (render priorità accanto al titolo), `style.css` (un colore per valore), test in `tests/todos.test.js`.

## Passi
1. API: definire i valori ammessi (`bassa`, `media`, `alta`) e il default (D1); aggiungere `Priority` al record `Todo` e a `NewTodo` (opzionale), serializzata come stringa.
2. API: validare la priorità in `POST /api/todos` (400 se non valida) e applicare il default se assente.
3. API: aggiungere l'endpoint dedicato per modificarla (forma in D3), con 404/400.
4. API: filtro `?priority=` su `GET /api/todos` e ordine dell'elenco secondo D2.
5. API: test (xUnit, `dotnet test api`) per creazione con/senza priorità, priorità non valida, modifica, filtro, 404; aggiornare `api/README.md`.
6. Web: `todos.js` invia `priority` in creazione, gestisce l'assenza del campo con il default, ordina secondo D2; test con `npm test --prefix web`.
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
- [ ] D1: Qual è la priorità di default dei todo creati senza `priority` (e di quelli senza campo lato web)? Proposta: `media` — fonti: richiesta nel thread ("non abbiamo deciso") vs spec.md
- [ ] D2: Qual è l'ordine dell'elenco? Oggi l'API ordina per id e il web per non fatti prima, poi id (`sortTodos`). Proposta: non fatti prima, poi priorità alta→bassa, poi id; l'ordine va applicato solo nel web o anche nell'API? — fonti: richiesta nel thread vs `Program.cs` (`OrderBy Id`) e `web/src/todos.js` (`sortTodos`)
- [ ] D3: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` (proposta, simile a `POST .../done`) o `POST`/`PATCH`? Il web deve anche permettere di cambiarla su un todo esistente o solo alla creazione? — fonti: richiesta nel thread ("menu alla creazione") vs `api/README.md` (convenzione `POST .../done`)
- [ ] D4: Un filtro `?priority=` con valore sconosciuto deve dare 400 (proposta) o elenco vuoto? Il web deve avere anche un filtro per priorità o solo l'API? — fonti: richiesta nel thread vs spec.md
- [ ] D5: I valori nel JSON sono le stringhe italiane `bassa`/`media`/`alta` (proposta, come nella richiesta) e vanno accettati senza distinzione di maiuscole? — fonti: richiesta nel thread vs convenzioni del contratto in CLAUDE.md
