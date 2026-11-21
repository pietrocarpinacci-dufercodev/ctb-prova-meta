# T-144b7b69 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, priorità in creazione, endpoint `PUT /api/todos/{id}/priority`, filtro `GET /api/todos?priority=`, test xUnit, `README.md`.
- web: `addTodo` invia la priorità, `setPriority` nuova in `todos.js`, menu nel form, etichetta colorata per ogni todo, CSS, test `node --test`.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - creazione con `priority` valida → 201 e valore restituito;
   - creazione senza `priority` → default `media`;
   - creazione con `priority` non valida → 400;
   - `PUT /api/todos/{id}/priority` → 200 e valore aggiornato, `title`/`done` invariati; 404 per id inesistente; 400 per valore non valido o assente;
   - `GET /api/todos?priority=alta` → solo i todo `alta`; valore non valido → 400.
   - Il record `Todo` usato nei test prende il nuovo campo. Il test di creazione esistente resta valido.
2. **api — implementazione** in `api/src/CtbProva.Api/Program.cs`:
   - `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`, nuovo `PriorityInput(string? Priority)`.
   - Valori ammessi in un solo array statico (`["bassa", "media", "alta"]`, D3) e default `media` (D1). Per tenere il valore come stringa del JSON, niente enum né converter.
   - Validazione con `Results.ValidationProblem` sul campo `priority`, come per `title`.
   - `TodoStore.Add(title, priority)` e `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`).
   - `MapGet("/api/todos", (string? priority, TodoStore store) => ...)`: senza parametro restituisce tutto, con un valore ammesso filtra, con un valore non ammesso risponde 400 (D5). L'ordine per id resta invariato (D6).
   - `MapPut("/api/todos/{id:int}/priority", ...)` (D2).
3. **api — documentazione**: in `api/README.md` aggiorna la tabella degli endpoint (body di creazione con `priority`, nuovo `PUT`, query `?priority=`).
4. **web — test prima** in `web/tests/todos.test.js`:
   - `addTodo` invia `priority` nel body;
   - `setPriority(id, priority, fetchImpl)` chiama `PUT /api/todos/{id}/priority` con il body giusto e solleva un errore se la risposta non è ok;
   - `sortTodos` resta invariato (test esistente).
5. **web — implementazione**:
   - `web/src/todos.js`: `PRIORITIES = ["bassa", "media", "alta"]`, `DEFAULT_PRIORITY = "media"`, `addTodo(title, priority = DEFAULT_PRIORITY, fetchImpl)`. L'ordine dei parametri cambia: aggiorna la chiamata in `app.js` e i test che passano `fakeFetch` come secondo argomento. Aggiungi `setPriority` come richiesto dal contratto: non serve alla UI chiesta, ma resta l'unico client dell'endpoint nel web (vedi D7).
   - `web/src/index.html`: `<select id="priority" aria-label="Priorità">` nel form, con le tre opzioni e `media` selezionata.
   - `web/src/app.js`: passa `prioritySelect.value` ad `addTodo` e rende per ogni todo uno `<span class="priority priority-<valore>">` con il testo della priorità.
   - `web/src/style.css`: tre colori (per esempio grigio/blu per bassa, ambra per media, rosso per alta), con contrasto sufficiente sul testo.
6. **meta-repo**: aggiorna l'elenco degli endpoint nel `CLAUDE.md` e in `.claude/agents/backend-dev.md` (aggiungi `/api/todos/{id}/priority` e il filtro `?priority=`).
7. **Verifica**: `dotnet test api` e `npm test --prefix web`. La UI non ha test automatici: va controllata a mano con API e `npm start --prefix web` (vedi Rischio).

## Fonti
- Richiesta nel thread Teams della feature (testo riportato in `spec.md`). Nessun allegato in `/work/inputs`.
- `CLAUDE.md` del meta-repo: struttura, comandi, regola sul contratto API ↔ web.
- `features/T-9dfe1d36/decisions.md`: sviluppo delegato all'agente `backend-dev` (D2, D6); nessun agente Frontend (D4).
- `.claude/agents/backend-dev.md`: convenzioni backend (400 per input non valido, 404 per risorsa assente).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.

## Rischio
Livello: verde
- Costi: nessuna dipendenza nuova né infrastruttura; dati in memoria.
- Complessità: bassa. Un campo, un endpoint e un filtro nell'API; un menu e un'etichetta nel web.
- Sicurezza: input nuovo (`priority` nel body e nella query) validato su un elenco chiuso, con 400 se non ammesso. Nel web la priorità va resa con `textContent`, come il titolo, mai con `innerHTML`.
- Accoppiamento: il contratto API ↔ web cambia in modo compatibile (campo aggiunto, `priority` facoltativo in creazione), ma va allineato nello stesso rilascio in `todos.js`, nei test e nella documentazione. La UI (menu e colori) non ha test automatici.

## Domande aperte
- [ ] D1: Quale priorità di default si assegna a un todo creato senza `priority`, e quale si preseleziona nel menu del web? Proposta: `media` — fonti: richiesta nel thread ("non abbiamo deciso la priorità di default") vs criteri di creazione in `spec.md`
- [ ] D2: Che forma ha l'endpoint dedicato per cambiare la priorità? Proposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }`, in alternativa `POST`, sul modello dell'esistente `POST /api/todos/{id}/done` — fonti: richiesta nel thread ("endpoint dedicato") vs convenzione degli endpoint in `api/README.md`
- [ ] D3: I valori nel JSON e nel codice sono quelli italiani `bassa`/`media`/`alta`, come nella richiesta, o quelli inglesi `low`/`medium`/`high`, con etichette italiane solo nella UI? Proposta: valori italiani, come chiesto — fonti: richiesta nel thread ("valori bassa, media e alta") vs regola delle lingue del progetto (codice, identificatori e testi nel codice in inglese)
- [ ] D4: I testi nuovi visibili all'utente (etichette del menu, messaggi di validazione dell'API) restano in italiano, come quelli esistenti ("Fatto", "Il titolo è obbligatorio."), o passano all'inglese? Proposta: UI in italiano, coerente con la pagina `lang="it"`; messaggi di validazione dell'API in inglese — fonti: regola delle lingue del progetto vs testi esistenti in `web/src/app.js` e `api/src/CtbProva.Api/Program.cs`
- [ ] D5: Con `GET /api/todos?priority=<valore non ammesso>` l'API risponde 400 o ignora il filtro e restituisce tutto? Proposta: 400 — fonti: richiesta nel thread (filtro per priorità, comportamento non specificato) vs convenzione "input non valido risponde 400" in `.claude/agents/backend-dev.md`
- [ ] D6: L'elenco va ordinato per priorità (API, web o entrambi) o resta nell'ordine attuale (API per id; web prima i todo da fare, poi per id)? Proposta: ordine invariato — fonti: richiesta nel thread ("non abbiamo deciso l'ordine dell'elenco") vs `TodoStore.All()` in `api/src/CtbProva.Api/Program.cs` e `sortTodos` in `web/src/todos.js`
- [ ] D7: Il web deve anche permettere di cambiare la priorità di un todo esistente e di filtrare l'elenco per priorità, o basta la parte chiesta (etichetta colorata e menu in creazione)? Proposta: solo la parte chiesta; `setPriority` in `todos.js` senza UI — fonti: richiesta nel thread (endpoint e filtro chiesti solo nell'API) vs regola del `CLAUDE.md` ("un cambio al contratto dell'API va riportato anche in `web/src/todos.js`")
- [ ] D8: La feature tocca anche il web, ma l'agente `backend-dev` è dedicato al .NET e non c'è un agente Frontend. Il lavoro sul web lo fa `backend-dev` o il runner senza agente? Proposta: `backend-dev` per entrambi i submodule, perché il `CLAUDE.md` lo chiede "per ogni lavoro sul codice" — fonti: `CLAUDE.md` del meta-repo (sezione Agente di sviluppo) vs `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend) e `description` di `.claude/agents/backend-dev.md`
