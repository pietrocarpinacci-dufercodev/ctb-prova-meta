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
7. **Esecuzione**: delega il lavoro su `api` e su `web` all'agente `backend-dev` (D8).
8. **Verifica**: `dotnet test api` e `npm test --prefix web`. La UI non ha test automatici: va controllata a mano con API e `npm start --prefix web` (vedi Rischio).

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
- [x] D1: Quale priorità di default si assegna a un todo creato senza `priority`, e quale si preseleziona nel menu del web? — risposta: `media` (proposta accettata nel thread)
- [x] D2: Che forma ha l'endpoint dedicato per cambiare la priorità? — risposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` (proposta accettata nel thread)
- [x] D3: I valori nel JSON e nel codice sono quelli italiani `bassa`/`media`/`alta` o quelli inglesi `low`/`medium`/`high`? — risposta: valori italiani `bassa`/`media`/`alta`, come chiesto; identificatori e commenti restano in inglese (proposta accettata nel thread)
- [x] D4: I testi nuovi visibili all'utente restano in italiano o passano all'inglese? — risposta: UI del web in italiano, coerente con la pagina `lang="it"`; messaggi di validazione nuovi dell'API in inglese (proposta accettata nel thread)
- [x] D5: Con `GET /api/todos?priority=<valore non ammesso>` l'API risponde 400 o ignora il filtro? — risposta: 400 (proposta accettata nel thread)
- [x] D6: L'elenco va ordinato per priorità o resta nell'ordine attuale? — risposta: ordine invariato, nell'API per id e nel web prima i todo da fare, poi per id (proposta accettata nel thread)
- [x] D7: Il web deve anche permettere di cambiare la priorità di un todo esistente e di filtrare l'elenco? — risposta: no, solo l'etichetta colorata e il menu in creazione; `setPriority` va in `todos.js` senza UI (proposta accettata nel thread)
- [x] D8: Il lavoro sul web lo fa `backend-dev` o il runner senza agente? — risposta: `backend-dev` per entrambi i submodule (proposta accettata nel thread)
