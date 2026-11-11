# T-4ab341d4 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, validazione in creazione, endpoint dedicato di modifica, filtro `?priority=` su `GET /api/todos`, test xUnit, README.
- web: priorità colorata accanto a ogni todo, `<select>` della priorità nel form di creazione, `addTodo` che invia la priorità, ordinamento (se D2 lo cambia), test `node --test`.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità valida, senza priorità (default D1), con priorità non valida (400); modifica valida (200), id inesistente (404), valore non valido (400); filtro valido, senza filtro, filtro non valido (400); ordine secondo D2.
2. **api — modello** in `Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`; un insieme statico dei valori ammessi (`bassa`, `media`, `alta`) usato da creazione, modifica e filtro. Valori stringa minuscoli, così il JSON resta `"priority": "alta"` senza convertitori.
3. **api — endpoint**: validazione in `POST /api/todos` (400 con `ValidationProblem` su `priority`, come per `title`); endpoint di modifica nella forma scelta in D4 con `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone`; parametro di query opzionale `priority` su `GET /api/todos`; ordine di `TodoStore.All()` secondo D2.
4. **api — README**: tabella endpoint aggiornata con campo, modifica e filtro.
5. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`; eventuale nuovo `sortTodos` secondo D2; costante dei valori ammessi.
6. **web — logica** in `web/src/todos.js`: `PRIORITIES = ["bassa", "media", "alta"]`, `addTodo(title, priority, fetchImpl)` che invia `{ title, priority }`; `sortTodos` aggiornato solo se D2 lo richiede.
7. **web — UI**: in `index.html` un `<select id="priority">` con `<label>` nel form, default D1; in `app.js` uno `<span class="priority priority-<valore>">` con il testo della priorità accanto al titolo e invio del valore scelto; in `style.css` tre colori (proposta: bassa grigio/verde, media ambra, alta rosso) con contrasto leggibile.
8. **Verifica**: `dotnet test api` e `npm test --prefix web`; prova manuale della pagina contro l'API se possibile nel container.
9. **Chiusura**: `decisions.md` aggiornato, descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md` (nomi presi da `.gitmodules`).

Delega (CLAUDE.md): il lavoro sull'api va a `backend-dev`; per il web vedi D7.

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
- [ ] D1: Qual è la priorità di default quando la creazione non la indica (proposta: `media`)? Vale anche come valore preselezionato nel menu del web. — fonti: richiesta del thread ("non abbiamo deciso la priorità di default") vs spec (creazione senza `priority`)
- [ ] D2: Come va ordinato l'elenco? Proposta: nel web prima i non fatti, poi priorità alta → media → bassa, poi id; l'API resta ordinata per id. In alternativa l'ordine per priorità anche nell'API, oppure nessun cambio. — fonti: richiesta del thread ("non abbiamo deciso l'ordine") vs `sortTodos` in `web/src/todos.js` e `TodoStore.All()` in `api`
- [ ] D3: I valori nel JSON restano esattamente `bassa`/`media`/`alta` minuscoli (e `Alta` viene rifiutato con 400), oppure vanno accettati senza distinzione di maiuscole o in inglese (`low`/`medium`/`high`)? — fonti: richiesta del thread vs convenzione dei nomi di campo in inglese nell'API (`title`, `done`)
- [ ] D4: Forma dell'endpoint dedicato: proposta `PUT /api/todos/{id}/priority` con corpo `{ "priority": "alta" }`; in alternativa `POST`, per coerenza con `POST /api/todos/{id}/done`. — fonti: richiesta del thread ("endpoint dedicato") vs pattern esistente in `api/src/CtbProva.Api/Program.cs`
- [ ] D5: Il filtro accetta una sola priorità (`?priority=alta`) o più valori (`?priority=alta&priority=media`)? Proposta: una sola. — fonti: richiesta del thread ("filtrare per priorità") vs nessuna convenzione di query nell'API
- [ ] D6: Il web deve anche permettere di cambiare la priorità di un todo esistente e di filtrare l'elenco, oppure solo mostrarla e sceglierla in creazione come chiesto? Proposta: solo quanto chiesto. — fonti: richiesta del thread (endpoint di modifica e filtro solo nell'API) vs richiesta del thread (web: solo visualizzazione e menu in creazione)
- [ ] D7: Il lavoro sul web va delegato anche lui a `backend-dev`, o lo fa direttamente l'esecutore senza agente? — fonti: `CLAUDE.md` del meta-repo ("backend-dev per ogni lavoro sul codice") vs `features/T-9dfe1d36/decisions.md` D4 (nessun agente Frontend) e `description` di `backend-dev` (Backend .NET)
