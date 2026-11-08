# T-59de7063 — Piano

## Submodule coinvolti
- api (ctb-prova-api): campo `priority` (`bassa`, `media`, `alta`) in `Todo`, default `media`; `NewTodo` accetta `priority` opzionale; nuovo `PATCH /api/todos/{id}/priority`; filtro `?priority=` su `GET /api/todos`; test in `TodoApiTests.cs`; tabella endpoint in `api/README.md`.
- web (ctb-prova-web): `web/src/todos.js` (`addTodo(title, priority)`, nuova `setPriority`), `web/src/app.js` (priorità accanto a ogni todo con classe di colore), `web/src/index.html` (menu priorità nel form, `media` preselezionata), `web/src/style.css` (tre colori), test in `web/tests/todos.test.js`.
- meta-repo: solo `features/T-59de7063/*` (CLAUDE.md già richiede di riportare i cambi di contratto in `web/src/todos.js` e nei test di entrambi).

## Passi
1. API: in `Program.cs` aggiungere `Priority` a `Todo` (valori `bassa|media|alta`, serializzati come stringhe minuscole). Default `media`.
2. API: `POST /api/todos` accetta `priority` opzionale; se assente usa `media`; se non valida risponde 400 con `ValidationProblem` su `priority`. Il confronto non distingue maiuscole/minuscole e il valore salvato è normalizzato in minuscolo.
3. API: `PATCH /api/todos/{id}/priority` con body `{ "priority": "alta" }`; restituisce il todo aggiornato; 404 se l'id non esiste, 400 se il valore non è valido (case-insensitive).
4. API: `GET /api/todos?priority=alta` filtra; senza parametro restituisce tutto; valore non valido → 400 (case-insensitive). L'ordine resta per id.
5. API: test (TDD) per default, creazione con priorità, case-insensitive, valore non valido, PATCH (ok/404/400), filtro (ok/non valido), ordine per id. `dotnet test api`.
6. API: aggiornare `api/README.md` (tabella endpoint, campo `priority`).
7. Web: `todos.js` — `addTodo` invia `priority`; `setPriority(id, priority)` con PATCH; `sortTodos` resta invariato (fatti in fondo, poi id; la priorità non conta, D6); test in `todos.test.js`. Nessun filtro per priorità nel web.
8. Web: `index.html`/`app.js`/`style.css` — `<select>` bassa/media/alta nel form (preselezionato `media`); etichetta di priorità accanto a ogni todo con colore distinto per livello (testo sempre visibile, non solo colore). `npm test --prefix web`.
9. Committare su `claude/T-59de7063` nei due submodule; descrizioni PR in `/work/out` (execute).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, contratto API/web, comandi di test).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`.
- Risposte del team nel thread (D1–D5). `features/_template/*`. Nessuna altra feature con `decisions.md`; nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: nessuno, dati in memoria, nessuna dipendenza nuova.
- Complessità: bassa; poche modifiche in due submodule piccoli.
- Sicurezza: bassa; la priorità è ristretta a tre valori e validata lato API; nel web si usa `textContent`.
- Accoppiamento: medio-basso; il contratto cambia in modo retro-compatibile (campo nuovo, parametro opzionale), ma API e web vanno rilasciati insieme per mostrare la priorità.

## Domande aperte
- [x] D1: priorità di default — risposta: `media`, anche preselezionata nel menu web.
- [x] D2: ordine dell'elenco — risposta: resta ordinato per id, in API e nel web.
- [x] D3: forma dell'endpoint dedicato — risposta: `PATCH /api/todos/{id}/priority`.
- [x] D4: valore non valido — risposta: 400; il confronto non distingue maiuscole e minuscole.
- [x] D5: filtro nel web — risposta: no, il filtro solo nell'API per ora.
- [x] D6: ordine nel web (`sortTodos`: fatti in fondo, poi id) — risposta: `sortTodos` resta com'è, con i fatti in fondo e poi per id; la priorità non cambia l'ordinamento.
