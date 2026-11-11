# T-4ab341d4 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, validazione in creazione, endpoint dedicato di modifica, filtro `?priority=` su `GET /api/todos`, test xUnit, README.
- web: priorità colorata accanto a ogni todo, `<select>` della priorità nel form di creazione, `addTodo` che invia la priorità, `sortTodos` per priorità, test `node --test`.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità valida, senza priorità (→ `media`, D1), con priorità non valida o maiuscola (`Alta`, `high` → 400, D3); `PUT /api/todos/{id}/priority` valida (200), id inesistente (404), valore assente o non valido (400); filtro `?priority=alta` valido, senza filtro (tutti), filtro non valido (400); elenco API ancora ordinato per id (D2).
2. **api — modello** in `Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`, `PriorityInput(string? Priority)`; un insieme statico dei valori ammessi (`bassa`, `media`, `alta`, confronto esatto) usato da creazione, modifica e filtro. Valori stringa minuscoli, così il JSON resta `"priority": "alta"` senza convertitori.
3. **api — endpoint**: validazione in `POST /api/todos` (400 con `ValidationProblem` su `priority`, come per `title`; default `media`); `PUT /api/todos/{id}/priority` con `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone`; parametro di query opzionale `priority` (un solo valore, D5) su `GET /api/todos`; `TodoStore.All()` resta ordinato per id.
4. **api — README**: tabella endpoint aggiornata con campo `priority`, default `media`, `PUT /api/todos/{id}/priority` e filtro `?priority=`.
5. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`; `sortTodos` mette prima i non fatti, poi alta → media → bassa, poi id (D2).
6. **web — logica** in `web/src/todos.js`: `PRIORITIES = ["bassa", "media", "alta"]`, `addTodo(title, priority, fetchImpl)` che invia `{ title, priority }`; `sortTodos` con il criterio di D2 (todo senza priorità trattati come `media`).
7. **web — UI** (solo visualizzazione e menu in creazione, D6): in `index.html` un `<select id="priority">` con `<label>` nel form, preselezionato su `media`; in `app.js` uno `<span class="priority priority-<valore>">` con il testo della priorità accanto al titolo (via `textContent`) e invio del valore scelto; in `style.css` tre colori: bassa verde, media ambra, alta rosso, con contrasto leggibile.
8. **Verifica**: `dotnet test api` e `npm test --prefix web`; prova manuale della pagina contro l'API se possibile nel container.
9. **Chiusura**: `decisions.md` aggiornato, descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md` (nomi presi da `.gitmodules`).

Delega (CLAUDE.md, D7): tutto il lavoro sul codice, api e web, va a `backend-dev`.

## Fonti
- Richiesta nel thread Teams della feature (nessun allegato in `/work/inputs`).
- `CLAUDE.md` del meta-repo: contratto API da riportare in `web/src/todos.js` e nei test di entrambi; delega a `backend-dev`.
- `features/T-9dfe1d36/decisions.md`: D4 (nessun agente Frontend), agente `backend-dev`.
- `.claude/agents/backend-dev.md`: convenzioni backend (400 per input non valido, 404 per risorsa assente).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.

## Rischio
Livello: verde
- Costi: nessuna dipendenza nuova, nessuna infrastruttura; due PR piccole più il meta-repo.
- Complessità: bassa; un campo, un endpoint, un parametro di query, un menu e tre stili.
- Sicurezza: input validato ai confini (valori ammessi chiusi, 400 altrimenti); nel web la priorità va scritta con `textContent` e la classe CSS costruita solo da valori ammessi.
- Accoppiamento: cambia il contratto API/web; il campo è additivo e la `priority` in creazione è opzionale, quindi un web vecchio continua a funzionare, ma i due submodule vanno rilasciati insieme per vedere la funzione.

## Domande aperte
- [x] D1: Qual è la priorità di default quando la creazione non la indica? — risposta: `media`, anche preselezionata nel menu del web (proposta accettata nel thread).
- [x] D2: Come va ordinato l'elenco? — risposta: nel web prima i non fatti, poi priorità alta → media → bassa, poi id; l'API resta ordinata per id (proposta accettata nel thread).
- [x] D3: Quali valori accetta `priority`? — risposta: solo `bassa`/`media`/`alta` minuscoli; ogni altro valore (anche `Alta`, `high`) risponde 400 (proposta accettata nel thread).
- [x] D4: Forma dell'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con corpo `{ "priority": "alta" }` (proposta accettata nel thread).
- [x] D5: Il filtro accetta una o più priorità? — risposta: una sola, `?priority=alta` (proposta accettata nel thread).
- [x] D6: Il web deve anche modificare la priorità e filtrare l'elenco? — risposta: no, solo visualizzazione colorata e menu in creazione (proposta accettata nel thread).
- [x] D7: Chi fa il lavoro sul web? — risposta: anche il web va delegato a `backend-dev`, scelta più semplice (un solo agente, regola del `CLAUDE.md` rispettata; il prompt dell'agente prevede già l'aggiornamento di `web/src/todos.js`); D4 di T-9dfe1d36 resta valida perché non si crea un agente Frontend (thread: "scegli tu la più semplice").
