# T-f841b620 — Piano

## Submodule coinvolti
- api: campo `priority` sul todo, priorità in creazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, ordinamento (D2), test e README.
- web: priorità accanto a ogni todo con colore e testo, menu di scelta nel form di creazione, `addTodo` invia la priorità, ordinamento (D2), test.

## Passi
1. api — test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità, creazione senza priorità (default D1), priorità non valida in creazione → 400, `PUT /api/todos/{id}/priority` ok / 404 / 400, filtro `?priority=` ok e valore non valido → 400, ordinamento (D2).
2. api — in `api/src/CtbProva.Api/Program.cs`:
   - enum `Priority { Low, Medium, High }` serializzato come stringa minuscola (`low|medium|high`) con `JsonStringEnumConverter` e naming policy camelCase configurati sulle opzioni JSON della minimal API;
   - `Todo(int Id, string Title, bool Done, Priority Priority)`, `NewTodo(string? Title, string? Priority)`;
   - validazione esplicita della stringa `priority` nell'handler (così il valore non valido dà `ValidationProblem` 400 sul campo `priority`, come per `title`, e non un errore di binding);
   - `TodoStore.Add(title, priority)`, nuovo `TodoStore.SetPriority(id, priority)` sullo stesso schema di `MarkDone`;
   - `MapPut("/api/todos/{id:int}/priority", ...)` con body `{ "priority": "..." }`;
   - `MapGet("/api/todos", (string? priority, ...))` con filtro e 400 sui valori non ammessi;
   - ordinamento di `All()` secondo D2.
3. api — aggiornare la tabella endpoint di `api/README.md` (campo `priority`, nuovo endpoint, filtro).
4. api — `dotnet test api` verde.
5. web — test prima in `web/tests/todos.test.js`: `addTodo` invia `priority`; `sortTodos` secondo D2; eventuale helper `priorityLabel` (low→Bassa, medium→Media, high→Alta).
6. web — `web/src/todos.js`: `addTodo(title, priority, fetchImpl)` invia `{ title, priority }`; costante `PRIORITIES` con valori ed etichette; `sortTodos` aggiornato per D2.
7. web — `web/src/index.html`: `<select id="priority">` con `<label>` nel form, opzioni Bassa/Media/Alta, default D1. `web/src/app.js`: legge il select in submit, mostra per ogni todo uno `<span class="priority priority-<valore>">` con il testo. `web/src/style.css`: tre colori (es. grigio/ambra/rosso) con contrasto sufficiente sul testo.
8. web — `npm test --prefix web` verde.
9. Aggiornare `features/T-f841b620/decisions.md` con le decisioni prese nell'execute; descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md`.

Fuori ambito (non richiesto): filtro per priorità e modifica della priorità nell'interfaccia web (vedi D6), persistenza, autenticazione.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul cambio di contratto API→web, agente `backend-dev`).
- `api/README.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend; l'agente `backend-dev` copre anche l'aggiornamento di `web/src/todos.js`).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: giallo
- Costi: bassi; poche decine di righe per submodule, nessuna dipendenza nuova (`JsonStringEnumConverter` è in `System.Text.Json`).
- Complessità: bassa; enum, una validazione, un endpoint, un filtro, un select e del CSS.
- Sicurezza: nessun dato sensibile; l'input nuovo (`priority` in body e query) va validato contro i tre valori ammessi.
- Accoppiamento: medio; cambia il contratto JSON tra api e web (nuovo campo, nuovo endpoint, firma di `addTodo`): le due PR vanno integrate insieme.

## Domande aperte
- [ ] D1: Qual è la priorità di default quando non viene indicata in creazione (bassa, media o alta)? Proposta: media. — fonti: richiesta del thread ("non abbiamo deciso la priorità di default") vs criterio di creazione senza `priority` in `spec.md`
- [ ] D2: In che ordine va mostrato l'elenco? Oggi l'API ordina per id (`TodoStore.All`) e il web per non fatti prima, poi id (`sortTodos`). Proposta: non fatti prima, poi priorità alta→bassa, poi id, nel web; l'API resta per id. — fonti: richiesta del thread ("non abbiamo deciso l'ordine dell'elenco") vs `api/src/CtbProva.Api/Program.cs` e `web/src/todos.js`
- [ ] D3: Nel JSON i valori sono in inglese (`low|medium|high`) con etichette italiane solo nel web, oppure in italiano (`bassa|media|alta`) come scritto nella richiesta? Proposta: inglese. — fonti: richiesta del thread (valori bassa, media, alta) vs regola delle lingue (codice in inglese)
- [ ] D4: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "high" }` (proposta) oppure `POST`, sullo stile di `POST /api/todos/{id}/done`? — fonti: richiesta del thread ("endpoint dedicato", forma non indicata) vs `api/README.md`
- [ ] D5: Un valore di priorità non ammesso (in creazione, nell'endpoint dedicato o nel filtro) dà 400 (proposta) oppure va ignorato/sostituito con il default? — fonti: richiesta del thread (non specifica) vs validazione esistente del titolo in `Program.cs` (400)
- [ ] D6: Il web deve usare anche il filtro per priorità e l'endpoint di modifica (es. un menu per cambiare la priorità di un todo esistente), o basta mostrarla e sceglierla in creazione come chiesto? Proposta: solo quanto chiesto. — fonti: richiesta del thread (API con filtro e modifica, web solo visualizzazione e creazione) vs `CLAUDE.md` (cambio di contratto da riportare in `web/src/todos.js`)
- [ ] D7: Il CLAUDE.md chiede di delegare tutto il codice a `backend-dev`, che è un agente .NET, e T-9dfe1d36 D4 esclude un agente Frontend: per la parte web (HTML/CSS/JS) va bene usare comunque `backend-dev`? Proposta: sì. — fonti: `CLAUDE.md` del meta-repo vs `features/T-9dfe1d36/decisions.md` D4
