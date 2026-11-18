# T-f841b620 — Piano

## Submodule coinvolti
- api: campo `priority` sul todo, priorità in creazione (default `medium`), endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test e README.
- web: priorità accanto a ogni todo con colore e testo, menu di scelta nel form di creazione, `addTodo` invia la priorità, ordinamento per priorità, test.

## Passi
1. api — test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità, creazione senza priorità (default D1), priorità non valida in creazione → 400, `PUT /api/todos/{id}/priority` ok / 404 / 400, filtro `?priority=` ok e valore non valido → 400.
2. api — in `api/src/CtbProva.Api/Program.cs`:
   - enum `Priority { Low, Medium, High }` serializzato come stringa minuscola (`low|medium|high`) con `JsonStringEnumConverter` e naming policy camelCase configurati sulle opzioni JSON della minimal API;
   - `Todo(int Id, string Title, bool Done, Priority Priority)`, `NewTodo(string? Title, string? Priority)`;
   - validazione esplicita della stringa `priority` nell'handler (così il valore non valido dà `ValidationProblem` 400 sul campo `priority`, come per `title`, e non un errore di binding);
   - `TodoStore.Add(title, priority)`, nuovo `TodoStore.SetPriority(id, priority)` sullo stesso schema di `MarkDone`;
   - `MapPut("/api/todos/{id:int}/priority", ...)` con body `{ "priority": "..." }`;
   - `MapGet("/api/todos", (string? priority, ...))` con filtro e 400 sui valori non ammessi;
   - `All()` resta ordinato per id (D2).
3. api — aggiornare la tabella endpoint di `api/README.md` (campo `priority`, nuovo endpoint, filtro).
4. api — `dotnet test api` verde.
5. web — test prima in `web/tests/todos.test.js`: `addTodo` invia `priority`; `sortTodos` con non fatti prima, poi alta→bassa, poi id (D2); helper `priorityLabel` (low→Bassa, medium→Media, high→Alta).
6. web — `web/src/todos.js`: `addTodo(title, priority, fetchImpl)` invia `{ title, priority }`; costante `PRIORITIES` con valori ed etichette; `sortTodos` aggiornato (D2).
7. web — `web/src/index.html`: `<select id="priority">` con `<label>` nel form, opzioni Bassa/Media/Alta, Media preselezionata (D1). `web/src/app.js`: legge il select in submit, mostra per ogni todo uno `<span class="priority priority-<valore>">` con il testo. `web/src/style.css`: tre colori (es. grigio/ambra/rosso) con contrasto sufficiente sul testo.
8. web — `npm test --prefix web` verde.
9. Aggiornare `features/T-f841b620/decisions.md` con le decisioni prese nell'execute; descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md`.

Tutti i passi, compresi quelli sul web, sono delegati al subagent `backend-dev` (D7).

Fuori ambito (non richiesto): filtro per priorità e modifica della priorità nell'interfaccia web (D6), persistenza, autenticazione.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul cambio di contratto API→web, agente `backend-dev`).
- `api/README.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend; l'agente `backend-dev` copre anche l'aggiornamento di `web/src/todos.js`).
- Risposte del thread alle domande D1–D7 (accettate le proposte del piano).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: giallo
- Costi: bassi; poche decine di righe per submodule, nessuna dipendenza nuova (`JsonStringEnumConverter` è in `System.Text.Json`).
- Complessità: bassa; enum, una validazione, un endpoint, un filtro, un select e del CSS.
- Sicurezza: nessun dato sensibile; l'input nuovo (`priority` in body e query) va validato contro i tre valori ammessi.
- Accoppiamento: medio; cambia il contratto JSON tra api e web (nuovo campo, nuovo endpoint, firma di `addTodo`): le due PR vanno integrate insieme.

## Domande aperte
- [x] D1: Qual è la priorità di default quando non viene indicata in creazione? — risposta: media (`medium`), anche come valore preselezionato nel menu del web.
- [x] D2: In che ordine va mostrato l'elenco? — risposta: nel web non fatti prima, poi priorità alta→bassa, poi id; l'API resta ordinata per id.
- [x] D3: Valori nel JSON in inglese o in italiano? — risposta: inglese (`low|medium|high`), etichette italiane (Bassa/Media/Alta) solo nel web.
- [x] D4: Forma dell'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "high" }`.
- [x] D5: Cosa fare con un valore di priorità non ammesso? — risposta: 400 (in creazione, nell'endpoint dedicato e nel filtro).
- [x] D6: Il web usa anche filtro e modifica della priorità? — risposta: no, solo visualizzazione e scelta in creazione, come chiesto.
- [x] D7: Usare `backend-dev` anche per la parte web? — risposta: sì.
