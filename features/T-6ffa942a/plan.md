# T-6ffa942a — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, validazione in creazione, endpoint dedicato per cambiarla, filtro `?priority=` su `GET /api/todos`, test xUnit e `README.md`.
- web: priorità colorata accanto a ogni todo, menu di scelta nel form di creazione, invio della priorità in `addTodo`, ordinamento per stato, priorità e id, test `node --test`.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità valida, creazione senza priorità (→ `media`), creazione con priorità non valida (→ 400), endpoint dedicato (200 / 400 / 404, titolo e stato invariati), filtro valido, filtro non valido (→ 400), elenco dell'API ancora ordinato per id (D2). Il test esistente che deserializza `Todo` va adattato al nuovo record.
2. **api — modello** in `api/src/CtbProva.Api/Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`, un record di input per l'endpoint dedicato e un insieme statico dei tre valori ammessi. La priorità resta una stringa validata contro l'insieme (niente enum e converter JSON: il contratto è già la stringa).
3. **api — store**: `TodoStore.Add(title, priority)`, nuovo `SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`), `All(string? priority)` con filtro, ordinamento per id invariato (D2).
4. **api — endpoint**: validazione di `priority` in `POST /api/todos` (`Results.ValidationProblem` come per il titolo), endpoint dedicato `POST /api/todos/{id}/priority` (D4), parametro di query `priority` su `GET /api/todos` con 400 se non valido.
5. **api — documentazione**: tabella endpoint e corpo di `POST /api/todos` in `api/README.md`. `dotnet test api` verde.
6. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority`, rifiuta una priorità non valida senza chiamare l'API, `sortTodos` aggiornato (da fare prima, poi alta → bassa, poi id; D2).
7. **web — logica** in `web/src/todos.js`: costante `PRIORITIES` con i tre valori, `addTodo(title, priority, fetchImpl)` con validazione, `sortTodos` secondo D2. Il cambio di firma di `addTodo` va riportato nel test esistente.
8. **web — interfaccia**: in `web/src/index.html` un `<select id="priority">` con `<label>` nel form, preimpostato su `media`; in `web/src/app.js` uno `<span class="priority priority-{valore}">` con il testo della priorità accanto al titolo e l'invio del valore del menu; in `web/src/style.css` tre colori (bassa verde, media arancio, alta rosso) con contrasto sufficiente. `npm test --prefix web` verde.
9. **meta-repo**: aggiornare l'elenco endpoint in `CLAUDE.md` e in `.claude/agents/backend-dev.md` con l'endpoint dedicato, poi i puntatori dei submodule `api` e `web`.
10. **Esecuzione**: il lavoro su `api` va delegato a `backend-dev` (regola del `CLAUDE.md`); `web` si fa senza agente (D7, T-9dfe1d36 D4).

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
- [x] D1: Quale priorità di default si assegna quando la creazione non la specifica, e il menu del web va preimpostato sullo stesso valore? — risposta: `media`, anche come valore preimpostato del menu (proposta accettata nel thread)
- [x] D2: In che ordine va l'elenco? — risposta: nel web (`sortTodos`) prima le cose da fare, poi priorità alta → bassa, poi id; l'API resta ordinata per id (proposta accettata nel thread)
- [x] D3: I valori nel JSON sono `bassa`/`media`/`alta` o `low`/`medium`/`high`? — risposta: `bassa`/`media`/`alta` letterali, come nella richiesta; scelta del bot perché la domanda non aveva proposta e il thread ha chiesto la più semplice: nessuna mappatura valori/etichette nel web. Sono valori del contratto chiesti dal team, non testi del codice; identificatori e commenti restano in inglese
- [x] D4: Forma dell'endpoint dedicato? — risposta: `POST /api/todos/{id}/priority` con corpo `{ "priority": "..." }` (proposta accettata nel thread)
- [x] D5: Filtro con valore non valido e maiuscole? — risposta: 400 per un valore non valido; confronto sensibile alle maiuscole, solo minuscolo (proposta accettata nel thread)
- [x] D6: Il web deve anche cambiare la priorità o filtrare? — risposta: no, solo mostrarla e sceglierla in creazione (proposta accettata nel thread)
- [x] D7: Il lavoro sul web si fa senza agente o con `backend-dev`? — risposta: `api` delegato a `backend-dev`, `web` senza agente; scelta del bot (la più semplice, la domanda non aveva proposta), coerente con T-9dfe1d36 D4
