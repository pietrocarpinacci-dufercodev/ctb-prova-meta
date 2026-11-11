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
7. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`; rifiuta priorità non valida senza chiamare l'API; senza priorità invia `media`. Niente `setPriority` né filtro nel web (D6).
8. **web — logica** in `web/src/todos.js`: costante `PRIORITIES = ["bassa", "media", "alta"]`, `DEFAULT_PRIORITY`, `addTodo(title, priority = DEFAULT_PRIORITY, fetchImpl)` (attenzione: cambia la posizione di `fetchImpl`, aggiornare il test esistente). `sortTodos` invariata (D2).
9. **web — UI**: in `index.html` un `<select id="priority">` con `<label>` nel form; in `app.js` uno `<span class="priority priority-<valore>">` col testo della priorità accanto al titolo e passaggio del valore scelto ad `addTodo`; in `style.css` tre colori (D7) con testo sempre visibile, così il significato non dipende solo dal colore. Eseguire `npm test --prefix web`.
10. **Chiusura**: aggiornare `decisions.md`, descrizioni PR in `/work/out` (`meta.md`, `api.md`, `web.md`), commit locali `[T-a9462771] ...` su `claude/T-a9462771` nei due submodule e nel meta-repo. Delega del codice secondo D8.

Fuori ambito (D2, D5, D6): persistenza, ordinamento per priorità, filtro su più valori, filtro e modifica della priorità dal web.

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
- [x] D1: Quale priorità di default se in creazione non viene indicata? — risposta: `media`, usata dall'API quando `priority` manca (niente 400) e preselezionata nel menu del web.
- [x] D2: Quale ordine per l'elenco? — risposta: invariato (API per id; web prima i da fare, poi per id); nessun ordinamento per priorità.
- [x] D3: Formato dei valori nel JSON? — risposta: stringhe italiane `bassa`/`media`/`alta`. La sensibilità alle maiuscole non aveva una proposta: vedi D9.
- [x] D4: Forma dell'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con corpo `{ "priority": "alta" }`.
- [x] D5: Filtro? — risposta: un solo valore `?priority=alta`; valore non valido → 400.
- [x] D6: Il web usa filtro e modifica della priorità? — risposta: no, solo visualizzazione e scelta in creazione.
- [x] D7: Colori delle priorità? — risposta: bassa grigio `#6b7383`, media ambra `#b54708`, alta rosso `#b42318`, sempre con il testo della priorità visibile.
- [ ] D8: Il lavoro sul web va fatto anche lui dall'agente `backend-dev` o senza agente? Nel piano precedente non c'era una proposta, quindi "va bene la tua proposta" non la copre. Proposta ora: `backend-dev` per entrambi i submodule (la decisione D4 di T-9dfe1d36 esclude solo la creazione di un agente Frontend, e il prompt di `backend-dev` già prevede di aggiornare `web/src/todos.js`) — fonti: `CLAUDE.md` del meta-repo ("usa backend-dev per ogni lavoro sul codice") vs ruolo Backend (.NET) di `.claude/agents/backend-dev.md` e `features/T-9dfe1d36/decisions.md` D4
- [ ] D9: I valori di priorità (corpo e query string) vanno confrontati in modo esatto, così `Alta` riceve 400, oppure ignorando le maiuscole? Proposta: confronto esatto, il più semplice e coerente con il web che invia sempre minuscolo — fonti: risposta a D3 nel thread vs domanda D3 del piano precedente, che non aveva una proposta su questo punto
