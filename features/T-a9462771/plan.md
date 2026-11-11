# T-a9462771 — Piano

## Submodule coinvolti
- api: campo `priority` sul todo, priorità in creazione con validazione, endpoint di modifica priorità, filtro `?priority=` sull'elenco, test e README.
- web: `addTodo` con priorità, menu nel form, etichetta colorata accanto a ogni todo, test.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità, default senza priorità, 400 su priorità non valida, modifica priorità (200/400/404), filtro per priorità (solo i todo di quel valore; 400 su valore non valido). Nota: la factory è condivisa tra i test, quindi i test sul filtro verificano che tutti i risultati abbiano la priorità richiesta e che il todo appena creato sia presente, senza contare il totale.
2. **api — modello** in `Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`, record `PriorityInput(string? Priority)`; insieme dei valori ammessi `bassa|media|alta` in un solo punto (array statico o helper `IsValid`). Valori come stringhe, nessun enum/convertitore JSON (D3).
3. **api — creazione**: `POST /api/todos` usa la priorità data o il default (D1); valore non valido → `ValidationProblem` su `priority`, come oggi per `title`.
4. **api — modifica**: `PUT /api/todos/{id:int}/priority` (D4) con `TodoStore.SetPriority(id, priority)` sullo stesso schema di `MarkDone` (`with { Priority = ... }`); 400 se non valida, 404 se assente.
5. **api — filtro**: `GET /api/todos` con parametro opzionale `string? priority`; se presente e valido filtra `store.All()`, se non valido 400 (D5). Ordine invariato per id (D2).
6. **api — README**: aggiornare la tabella endpoint e il corpo del POST. Eseguire `dotnet test api`.
7. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`; rifiuta priorità non valida senza chiamare l'API; `setPriority`/`fetchTodos(priority)` solo se confermati in D6.
8. **web — logica** in `web/src/todos.js`: costante `PRIORITIES = ["bassa", "media", "alta"]`, `DEFAULT_PRIORITY`, `addTodo(title, priority = DEFAULT_PRIORITY, fetchImpl)` (attenzione: cambia la posizione di `fetchImpl`, aggiornare il test esistente). `sortTodos` invariata (D2).
9. **web — UI**: in `index.html` un `<select id="priority">` con `<label>` nel form; in `app.js` uno `<span class="priority priority-<valore>">` col testo della priorità accanto al titolo e passaggio del valore scelto ad `addTodo`; in `style.css` tre colori (D7) con testo sempre visibile, così il significato non dipende solo dal colore. Eseguire `npm test --prefix web`.
10. **Chiusura**: aggiornare `decisions.md`, descrizioni PR in `/work/out` (`meta.md`, `api.md`, `web.md`), commit locali `[T-a9462771] ...` su `claude/T-a9462771` nei due submodule e nel meta-repo. Delega del codice secondo D8.

Fuori ambito (non richiesto): persistenza, ordinamento per priorità lato API, filtro multiplo, modifica della priorità dal web (salvo D6).

## Fonti
- Richiesta nel thread Teams (messaggio del job plan).
- `CLAUDE.md` del meta-repo: contratto API/web da allineare in `web/src/todos.js` e nei test; comandi `dotnet test api`, `npm test --prefix web`; delega a `backend-dev`.
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend) e `.claude/agents/backend-dev.md`.
- Codice: `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: giallo
- Costi: bassi, nessuna dipendenza nuova; due PR di submodule più il meta-repo.
- Complessità: bassa, poche righe per lato su codice esistente piccolo.
- Sicurezza: bassa; nuovo input da validare ai confini (corpo del POST/PUT e query string), coperto dai 400.
- Accoppiamento: medio, cambia il contratto tra `api` e `web` (nuovo campo, nuovo endpoint, firma di `addTodo`): le due PR vanno unite insieme.

## Domande aperte
- [ ] D1: Quale priorità di default se in creazione non viene indicata (proposta: `media`, preselezionata anche nel menu del web)? E in assenza di priorità l'API deve usare il default o rispondere 400? — fonti: richiesta nel thread ("non abbiamo deciso la priorità di default") vs criteri di creazione in spec.md
- [ ] D2: Quale ordine per l'elenco? Proposta: invariato (API per id; web prima i da fare, poi per id). Alternativa: per priorità (alta→bassa) dentro i da fare. E l'eventuale ordine va applicato nell'API, nel web o in entrambi? — fonti: richiesta nel thread ("non abbiamo deciso l'ordine dell'elenco") vs `TodoStore.All()` in `api/src/CtbProva.Api/Program.cs` e `sortTodos` in `web/src/todos.js`
- [ ] D3: Formato dei valori nel JSON: stringhe italiane `bassa`/`media`/`alta` (proposta, come nella richiesta), inglesi `low`/`medium`/`high` o numeri? Il confronto deve essere case-insensitive? — fonti: richiesta nel thread (valori in italiano) vs nome del campo in inglese `priority`
- [ ] D4: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con corpo `{ "priority": "alta" }` (proposta) oppure `POST`, come l'esistente `/api/todos/{id}/done`? — fonti: richiesta nel thread vs convenzione di `api/README.md`
- [ ] D5: Filtro: un solo valore `?priority=alta` (proposta) o più valori? Un valore non valido deve dare 400 (proposta) o un elenco vuoto? — fonti: richiesta nel thread (filtro non specificato)
- [ ] D6: Il web deve usare anche il filtro per priorità e la modifica della priorità di un todo esistente, o solo mostrarla e sceglierla in creazione (proposta: solo quanto chiesto)? — fonti: richiesta nel thread (parte web) vs parte API della stessa richiesta
- [ ] D7: Colori delle priorità nel web: proposta bassa grigio `#6b7383`, media ambra `#b54708`, alta rosso `#b42318` (stesso rosso di `#error`, che potrebbe confondersi con un errore). Vanno bene? — fonti: richiesta nel thread ("un colore diverso") vs `web/src/style.css`
- [ ] D8: Il lavoro sul web va fatto anche lui dall'agente `backend-dev` o senza agente? — fonti: `CLAUDE.md` del meta-repo ("usa backend-dev per ogni lavoro sul codice") vs `features/T-9dfe1d36/decisions.md` D4 (nessun agente Frontend) e ruolo Backend (.NET) di `.claude/agents/backend-dev.md`
