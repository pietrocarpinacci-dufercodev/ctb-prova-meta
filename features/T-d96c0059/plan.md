# T-d96c0059 — Piano

## Submodule coinvolti
- api: campo `priority` in `Todo`/`NewTodo`/`TodoStore`, validazione, endpoint di modifica priorità, filtro `?priority=` su `GET /api/todos`, test in `TodoApiTests.cs`, tabella endpoint in `README.md`.
- web: `web/src/todos.js` (creazione con priorità, eventuale ordinamento), `app.js` e `index.html` (menu priorità, etichetta colorata), stile, test in `web/tests/todos.test.js`, `README.md`.

## Passi
1. API (TDD): aggiungere `Priority` al record `Todo` (stringa `bassa|media|alta`, validata in un unico punto) e `Priority` opzionale a `NewTodo`; default secondo D1.
2. API: `POST /api/todos` valida `priority` (400 se non valido).
3. API: endpoint dedicato di modifica (forma proposta: `PUT /api/todos/{id}/priority` con `{ "priority": "..." }`, 404/400 come da spec; vedi D3).
4. API: filtro `GET /api/todos?priority=...` (400 se non valido); ordine dell'elenco secondo D2.
5. API: test per creazione con/senza priorità, valore non valido, modifica, 404, filtro; aggiornare `api/README.md`.
6. Web: `addTodo(title, priority)` invia `priority`; menu `<select>` nel form con default coerente con l'API; etichetta con classe per colore (`bassa`/`media`/`alta`) accanto a ogni todo; ordinamento in `sortTodos` solo se deciso in D2.
7. Web: test in `todos.test.js` (invio priorità nel body, ordinamento); aggiornare `web/README.md`.
8. Verifica: `dotnet test api` e `npm test --prefix web`. Il contratto cambia in modo compatibile (campo nuovo opzionale): consegnare api e web insieme.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API/web).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`.
- Nessun allegato in `/work/inputs`; nessuna altra feature con `decisions.md`.

## Rischio
Livello: verde
- Costi: modesti, modifica piccola in due submodule.
- Complessità: bassa; dati in memoria, nessuna migrazione.
- Sicurezza: validare `priority` (lista chiusa) lato API; nel web usare `textContent`, non `innerHTML`.
- Accoppiamento: il contratto API cambia (campo nuovo, query param, endpoint) e va riportato in `web/src/todos.js` e nei test di entrambi; modifica retrocompatibile.

## Domande aperte
- [ ] D1: Quale priorità di default per i todo creati senza `priority` (media? bassa?) — fonti: richiesta nel thread (non decisa) vs spec
- [ ] D2: Quale ordine per l'elenco (per id come ora, oppure alta→bassa; e come si combina con il criterio attuale "non fatti prima" del web)? Se per priorità, lo applica l'API o solo il web? — fonti: richiesta nel thread (non decisa) vs `web/src/todos.js` (`sortTodos`) e `Program.cs` (`All()` per id)
- [ ] D3: Endpoint di modifica: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` va bene (in linea con `/done`, che usa POST)? PUT o POST? — fonti: richiesta nel thread ("endpoint dedicato") vs `api/README.md` (convenzione POST per `/done`)
- [ ] D4: Valori in italiano minuscolo (`bassa`, `media`, `alta`) anche nel JSON, case-sensitive? — fonti: richiesta nel thread vs convenzioni API (campi in inglese)
- [ ] D5: Il web deve permettere anche di cambiare la priorità di un todo esistente e di filtrare per priorità? La richiesta cita solo il menu in creazione; senza indicazioni non lo implemento — fonti: richiesta nel thread
