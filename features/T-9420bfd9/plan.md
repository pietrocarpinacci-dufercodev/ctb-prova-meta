# T-9420bfd9 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo` e `NewTodo` con validazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, ordine dell'elenco (D2), test xUnit, `README.md`
- web: menu della priorità nel form, priorità colorata accanto a ogni todo, `addTodo` che manda la priorità, `sortTodos` allineato a D2, test `node --test`

## Passi
1. **api, test prima (TDD)**: in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` scrivere i test per creazione con e senza `priority` (default D1), priorità non valida → 400, `PUT /api/todos/{id}/priority` (200, 400, 404), filtro `GET /api/todos?priority=alta`, filtro non valido → 400, ordine dell'elenco (D2). Aggiornare il record `Todo` usato dai test.
2. **api, codice** in `api/src/CtbProva.Api/Program.cs`:
   - `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`;
   - un solo insieme dei valori ammessi (`bassa`, `media`, `alta`), usato per validare creazione, modifica e filtro (risposta 400 con `Results.ValidationProblem`, come per il titolo);
   - `TodoStore.Add(title, priority)` e `TodoStore.SetPriority(id, priority)`, sullo stesso modello di `MarkDone`;
   - `MapPut("/api/todos/{id:int}/priority", ...)` con body `{ "priority": "..." }` (forma da confermare in D4);
   - `GET /api/todos` con `string? priority` opzionale da query string;
   - ordine dell'elenco in `All()` secondo D2.
   - Valori come stringhe minuscole, senza enum né converter, salvo diversa risposta a D3.
3. **api, documentazione**: tabella degli endpoint in `api/README.md` (nuovo endpoint, campo `priority` in creazione, filtro). Eseguire `dotnet test api`.
4. **web, test prima** in `web/tests/todos.test.js`: `addTodo` manda `priority` nel body; `sortTodos` segue D2; (se serve) la funzione che dà la classe CSS ignora i valori sconosciuti.
5. **web, codice**:
   - `web/src/todos.js`: `addTodo(title, priority, fetchImpl)` manda `{ title, priority }`; costante `PRIORITIES = ["bassa", "media", "alta"]`; `sortTodos` secondo D2;
   - `web/src/index.html`: `<select id="priority">` con `<label>` e le tre opzioni, preselezionata su D1;
   - `web/src/app.js`: legge la select al submit; in `render` aggiunge uno `<span class="priority priority-<valore>">` con il testo della priorità (il testo resta, così il dato non è affidato solo al colore);
   - `web/src/style.css`: tre colori per `.priority-bassa`, `.priority-media`, `.priority-alta` con contrasto sufficiente sul fondo `#f6f7f9`.
   - Fuori ambito, salvo diversa risposta a D6: filtro e modifica della priorità dal web.
   - Eseguire `npm test --prefix web`.
6. **meta-repo**: aggiornare l'elenco degli endpoint in `CLAUDE.md` (aggiungere `/api/todos/{id}/priority` e `?priority=`), `decisions.md` con le risposte, commit dei puntatori di `api` e `web`, descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md`.
7. **Verifica manuale (fuori dal container, se possibile)**: avviare API e web, creare todo con le tre priorità, controllare colori e ordine.

## Fonti
- Richiesta nel thread Teams della feature T-9420bfd9 (nessun allegato in `/work/inputs`)
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API ↔ web, regola su `backend-dev`)
- `.claude/agents/backend-dev.md`
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend)
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`

## Rischio
Livello: verde
- Costi: lavoro piccolo su due submodule, nessuna dipendenza nuova, nessuna infrastruttura.
- Complessità: un campo, un endpoint e un filtro in una minimal API; select e badge nel web. Dati in memoria, quindi niente migrazioni.
- Sicurezza: nuovo input da validare contro un insieme chiuso di valori (body e query string); il web inserisce il testo con `textContent`, quindi niente HTML iniettato.
- Accoppiamento: cambia il contratto tra `api` e `web` (nuovo campo nella risposta e nella creazione). Il campo è opzionale in creazione, quindi un client che non lo manda continua a funzionare; le due PR vanno comunque rilasciate insieme.

## Domande aperte
- [ ] D1: Qual è la priorità di default quando la creazione non la indica (API) e quale opzione è preselezionata nel menu del web? Proposta: `media`. — fonti: richiesta nel thread ("non abbiamo deciso la priorità di default") vs spec.md (criterio su `POST /api/todos` senza `priority`)
- [ ] D2: In che ordine va restituito e mostrato l'elenco? Oggi l'API ordina per id e il web mette prima i non fatti, poi per id. Proposta: lasciare l'API per id e nel web ordinare per non fatti, poi priorità (alta → bassa), poi id. — fonti: richiesta nel thread ("non abbiamo deciso l'ordine") vs `api/.../Program.cs` (`All()` per id) e `web/src/todos.js` (`sortTodos`)
- [ ] D3: Nel JSON la priorità è la stringa minuscola `"bassa"`/`"media"`/`"alta"` (proposta), un valore inglese (`low`/`medium`/`high`) o un numero? E il confronto è sensibile alle maiuscole (proposta: sì, valori solo minuscoli)? — fonti: richiesta nel thread (valori in italiano) vs nessuna convenzione nel codice esistente
- [ ] D4: Quale forma per l'endpoint dedicato? Proposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }`, risposta 200 con il todo. In alternativa `POST`, come il già esistente `/done`, oppure `PATCH /api/todos/{id}`. — fonti: richiesta nel thread ("endpoint dedicato") vs `api/README.md` (`POST /api/todos/{id}/done`)
- [ ] D5: Un filtro con valore non valido (`GET /api/todos?priority=urgente`) risponde 400 (proposta) o restituisce un elenco vuoto? — fonti: richiesta nel thread (filtro per priorità) vs `.claude/agents/backend-dev.md` (input non valido → 400)
- [ ] D6: Il web deve usare anche il filtro e la modifica della priorità (menu di filtro, cambio di priorità su un todo esistente)? La richiesta per il web cita solo visualizzazione e scelta in creazione; proposta: fuori ambito. — fonti: richiesta nel thread (parte web) vs `CLAUDE.md` del meta-repo ("un cambio al contratto va riportato anche in `web/src/todos.js`")
- [ ] D7: Nell'execute la parte web va delegata anch'essa a `backend-dev` (agente Backend .NET) o va svolta senza agente? Proposta: `backend-dev` per `api`, web senza agente, e lo si scrive nell'esito. — fonti: `CLAUDE.md` del meta-repo ("backend-dev per ogni lavoro sul codice") vs `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend) e `.claude/agents/backend-dev.md` (ruolo Backend)
- [ ] D8: Quali colori per le tre priorità? Proposta: bassa grigio/verde, media ambra, alta rosso, con il testo sempre visibile accanto al colore. — fonti: richiesta nel thread ("un colore diverso") vs `web/src/style.css` (nessuna palette definita)
