# T-6ffa942a — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, validazione in creazione, endpoint dedicato per cambiarla, filtro `?priority=` su `GET /api/todos`, test xUnit e `README.md`.
- web: priorità colorata accanto a ogni todo, menu di scelta nel form di creazione, invio della priorità in `addTodo`, ordinamento secondo D2, test `node --test`.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità valida, creazione senza priorità (→ `<default>`), creazione con priorità non valida (→ 400), endpoint dedicato (200 / 400 / 404, titolo e stato invariati), filtro valido, filtro non valido (→ 400), ordine dell'elenco secondo D2. Il test esistente che deserializza `Todo` va adattato al nuovo record.
2. **api — modello** in `api/src/CtbProva.Api/Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`, un record di input per l'endpoint dedicato e un insieme statico dei tre valori ammessi. La priorità resta una stringa validata contro l'insieme (niente enum e converter JSON: il contratto è già la stringa).
3. **api — store**: `TodoStore.Add(title, priority)`, nuovo `SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`), `All(string? priority)` con filtro e ordinamento secondo D2.
4. **api — endpoint**: validazione di `priority` in `POST /api/todos` (`Results.ValidationProblem` come per il titolo), endpoint dedicato secondo D4, parametro di query `priority` su `GET /api/todos` con 400 se non valido.
5. **api — documentazione**: tabella endpoint e corpo di `POST /api/todos` in `api/README.md`. `dotnet test api` verde.
6. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`, rifiuta una priorità non valida senza chiamare l'API, `sortTodos` aggiornato secondo D2.
7. **web — logica** in `web/src/todos.js`: costante `PRIORITIES` con i tre valori, `addTodo(title, priority, fetchImpl)` con validazione, `sortTodos` secondo D2. Il cambio di firma di `addTodo` va riportato nel test esistente.
8. **web — interfaccia**: in `web/src/index.html` un `<select id="priority">` con `<label>` nel form, preimpostato su `<default>`; in `web/src/app.js` uno `<span class="priority priority-<valore>">` con il testo della priorità accanto al titolo e l'invio del valore del menu; in `web/src/style.css` tre colori (proposta: bassa grigio/verde, media arancio, alta rosso) con contrasto sufficiente. `npm test --prefix web` verde.
9. **meta-repo**: aggiornare l'elenco endpoint in `CLAUDE.md` e in `.claude/agents/backend-dev.md` con l'endpoint dedicato, poi i puntatori dei submodule `api` e `web`.
10. **Esecuzione**: il lavoro su `api` va delegato a `backend-dev` (regola del `CLAUDE.md`); per `web` non esiste un agente Frontend (T-9dfe1d36 D4), quindi si fa senza agente oppure con `backend-dev` se D7 lo conferma.

## Fonti
- Richiesta nel thread del job plan (nessun allegato in `/work/inputs`).
- `CLAUDE.md` del meta-repo: contratto web/API, comandi, delega a `backend-dev`.
- `.claude/agents/backend-dev.md`: convenzioni backend (400 per input non valido, 404 per risorsa assente).
- `features/T-9dfe1d36/decisions.md`: D4 (nessun agente Frontend), D6/D7 (execute con `backend-dev`).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`.

## Rischio
Livello: giallo
- Costi: bassi, due submodule piccoli, nessuna dipendenza nuova.
- Complessità: bassa, un campo, un endpoint e un filtro su uno store in memoria.
- Sicurezza: nessun dato sensibile; l'unico punto è la validazione dei valori di `priority` ai confini (body e query), coperta da test.
- Accoppiamento: medio, cambia il contratto JSON condiviso tra `api` e `web` (nuovo campo, nuova firma di `addTodo`) e le due PR vanno pubblicate insieme; da qui il giallo.

## Domande aperte
- [ ] D1: Quale priorità di default si assegna quando la creazione non la specifica (proposta: media), e il menu del web va preimpostato sullo stesso valore? — fonti: richiesta nel thread ("non abbiamo deciso la priorità di default") vs spec (criterio sulla creazione senza `priority`)
- [ ] D2: In che ordine va l'elenco? Oggi l'API ordina per id e il web per stato (da fare prima) e poi per id; proposta: da fare prima, poi priorità alta → bassa, poi id, solo nel web (`sortTodos`) lasciando l'API per id. — fonti: richiesta nel thread ("non abbiamo deciso l'ordine dell'elenco") vs `web/src/todos.js` `sortTodos` e `TodoStore.All`
- [ ] D3: I valori nel JSON sono letteralmente `bassa`/`media`/`alta` come nella richiesta, oppure `low`/`medium`/`high` con le etichette italiane solo nell'interfaccia web? — fonti: richiesta nel thread (valori bassa, media e alta) vs regola delle lingue del progetto (codice e testi nel codice in inglese)
- [ ] D4: Forma dell'endpoint dedicato: `POST /api/todos/{id}/priority` con corpo `{ "priority": "..." }`, coerente con `POST /api/todos/{id}/done` (proposta), oppure `PUT`/`PATCH`? — fonti: richiesta nel thread ("endpoint dedicato") vs `api/README.md` (convenzione esistente `POST .../done`)
- [ ] D5: Un valore di `priority` non valido nel filtro `GET /api/todos?priority=` deve dare 400 (proposta, coerente con la validazione ai confini) oppure essere ignorato restituendo tutto? E il confronto dei valori è sensibile alle maiuscole (proposta: sì, solo minuscolo)? — fonti: richiesta nel thread (filtro per priorità) vs `.claude/agents/backend-dev.md` (input non valido → 400)
- [ ] D6: Il web deve anche permettere di cambiare la priorità di un todo esistente o di filtrare l'elenco, o solo mostrarla e sceglierla in creazione come chiesto (proposta: solo quanto chiesto)? — fonti: richiesta nel thread (endpoint e filtro solo nell'API) vs `CLAUDE.md` (un cambio di contratto va riportato in `web/src/todos.js`)
- [ ] D7: Il lavoro sul web nell'execute si fa senza agente o lo si delega comunque a `backend-dev`? — fonti: `CLAUDE.md` (usare `backend-dev` per ogni lavoro sul codice) vs `features/T-9dfe1d36/decisions.md` D4 (nessun agente Frontend) e descrizione di `backend-dev` (Backend .NET)
