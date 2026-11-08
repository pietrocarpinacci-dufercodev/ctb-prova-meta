# T-d96c0059 — Piano

## Submodule coinvolti
- api: campo `priority` in `Todo`/`NewTodo`/`TodoStore`, validazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test in `TodoApiTests.cs`, tabella endpoint in `README.md`.
- web: `web/src/todos.js` (creazione con priorità, ordinamento), `app.js` e `index.html` (menu priorità, etichetta colorata), stile, test in `web/tests/todos.test.js`, `README.md`.

## Passi
1. API (TDD): aggiungere `Priority` al record `Todo` (stringa `bassa|media|alta`, validata in un unico punto, confronto case-sensitive) e `Priority` opzionale a `NewTodo`; default `media`.
2. API: `POST /api/todos` valida `priority` (400 se non valido).
3. API: `PUT /api/todos/{id}/priority` con `{ "priority": "..." }`: 200 con il todo aggiornato, 400 se non valido, 404 se non esiste.
4. API: filtro `GET /api/todos?priority=...` (400 se non valido). L'ordine dell'API resta per id.
5. API: test per creazione con/senza priorità, valore non valido, modifica, 404, filtro; aggiornare `api/README.md`.
6. Web: `addTodo(title, priority)` invia `priority`; `<select>` nel form con default `media`; etichetta con classe per colore (`bassa`/`media`/`alta`) accanto a ogni todo, scritta con `textContent`; `sortTodos` ordina per non fatti prima, poi priorità alta→bassa, poi id.
7. Web: test in `todos.test.js` (priorità nel body, ordinamento); aggiornare `web/README.md`.
8. Verifica: `dotnet test api` e `npm test --prefix web`. Il contratto cambia in modo compatibile (campo nuovo opzionale): consegnare api e web insieme.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API/web).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`.
- Risposta del team nel thread: proposte del piano accettate per tutte le domande.
- Nessun allegato in `/work/inputs`; nessuna altra feature con `decisions.md`.

## Rischio
Livello: verde
- Costi: modesti, modifica piccola in due submodule.
- Complessità: bassa; dati in memoria, nessuna migrazione.
- Sicurezza: validare `priority` (lista chiusa) lato API; nel web usare `textContent`, non `innerHTML`.
- Accoppiamento: il contratto API cambia (campo nuovo, query param, endpoint) e va riportato in `web/src/todos.js` e nei test di entrambi; modifica retrocompatibile.

## Domande aperte
- [x] D1: Priorità di default — risposta: `media`
- [x] D2: Ordine dell'elenco — risposta: l'API resta per id; il web ordina non fatti prima, poi priorità alta→bassa, poi id
- [x] D3: Endpoint di modifica — risposta: `PUT /api/todos/{id}/priority` con `{ "priority": "..." }`
- [x] D4: Valori nel JSON — risposta: `bassa`, `media`, `alta`, minuscolo, case-sensitive
- [x] D5: Web con modifica e filtro su todo esistenti — risposta: no, solo menu in creazione ed etichetta; filtro solo nell'API
