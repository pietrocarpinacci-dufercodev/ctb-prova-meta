# T-b031fc12 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`, priorità in creazione, endpoint dedicato per modificarla, filtro `?priority=` sull'elenco, test xUnit, README.
- web: priorità colorata accanto a ogni todo, menu di scelta nel modulo di creazione, `addTodo` che invia la priorità, test Node.

## Passi
1. **api — test prima (TDD)** in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: creazione con priorità valida (201, valore restituito), creazione senza priorità (`media`, D1), priorità `Alta` in maiuscolo (400, D6), priorità non valida in creazione (400, todo non creato), endpoint dedicato (200 e valore aggiornato, 400 se valore non valido o mancante, 404 se id inesistente), `GET /api/todos?priority=alta` (solo quei todo), filtro non valido (400). Il record `Todo` usato nei test prende il campo `Priority`. Nota: i test condividono lo store della `WebApplicationFactory`, quindi il test del filtro verifica che ogni elemento restituito abbia la priorità richiesta e che il todo appena creato ci sia, non un conteggio esatto.
2. **api — codice** in `api/src/CtbProva.Api/Program.cs`, nella forma minimal API già presente:
   - `record Todo(int Id, string Title, bool Done, string Priority)`; `NewTodo(string? Title, string? Priority)`; nuovo `record PriorityInput(string? Priority)`.
   - Valori ammessi in un array statico `["bassa", "media", "alta"]`, confronto esatto in minuscolo (D6); la priorità resta una stringa nel JSON, senza enum né convertitori.
   - `POST /api/todos`: priorità assente → `media` (D1); non valida → `Results.ValidationProblem` su `priority`, come per il titolo.
   - Endpoint dedicato `PUT /api/todos/{id:int}/priority` (D3): 400 se non valida, 404 se il todo non esiste, 200 con il todo aggiornato; `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone`.
   - `GET /api/todos` con parametro opzionale `string? priority`: assente → tutti; non valido → 400; valido → filtro. Ordine invariato per id (D2).
3. **api — documentazione**: `api/README.md` con il campo `priority`, il corpo di `POST /api/todos`, il nuovo endpoint e il filtro.
4. **web — test prima** in `web/tests/todos.test.js`: `addTodo` invia `priority` nel corpo; `addTodo` rifiuta una priorità non valida senza chiamare l'API; eventuale helper puro per l'etichetta/classe CSS della priorità.
5. **web — codice**:
   - `web/src/todos.js`: esporta `PRIORITIES = ["bassa", "media", "alta"]` e `DEFAULT_PRIORITY = "media"` (D1); `addTodo(title, priority = DEFAULT_PRIORITY, fetchImpl)` valida e invia `{ title, priority }`. Attenzione: oggi `fetchImpl` è il secondo parametro, quindi vanno aggiornati i test esistenti che lo passano in seconda posizione. `sortTodos` invariato (D2).
   - `web/src/index.html`: `<label>` + `<select id="priority">` con le tre opzioni, preselezionato su `media`. Nessun filtro né modifica della priorità nel web (D4).
   - `web/src/app.js`: legge il valore del select in creazione; in `render` aggiunge uno `<span class="priority priority-<valore>">` con il testo della priorità.
   - `web/src/style.css`: tre colori per `.priority-bassa`, `.priority-media`, `.priority-alta` (grigio-blu, ambra, rosso, con contrasto leggibile; D5).
6. **web — documentazione**: `web/README.md` solo se serve citare il menu (probabilmente no).
7. **meta-repo**: aggiornare l'elenco endpoint nel `CLAUDE.md` e in `.claude/agents/backend-dev.md` (nuovo endpoint e filtro `?priority=`), aggiornare `decisions.md` della feature.
8. **Verifica**: `dotnet test api` e `npm test --prefix web`; controllo manuale della pagina solo se il container lo permette (altrimenti indicarlo nelle PR come test non eseguito).
9. **Esecuzione**: per CLAUDE.md il lavoro sul codice va delegato al subagent `backend-dev`, anche per il web (non esiste un agente Frontend, D4 di T-9dfe1d36).

## Fonti
- Richiesta nel thread Teams della feature T-b031fc12 (messaggio del job plan, 2026-11-12) e risposta del thread che accetta tutte le proposte per D1–D6.
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API → `web/src/todos.js` e test di entrambi, delega a `backend-dev`).
- `.claude/agents/backend-dev.md` (validazione ai confini: 400/404; TDD).
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend; D6/D7: delega a `backend-dev`).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`, `web/README.md`.
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: nessuna dipendenza nuova, nessun servizio esterno; dati in memoria.
- Complessità: bassa; un campo, un endpoint, un parametro di query e un select, nella forma già usata dal codice.
- Sicurezza: input validato ai confini (valori ammessi fissi, 400 altrimenti); il testo della priorità nel web va in `textContent`, niente HTML iniettato.
- Accoppiamento: cambia il contratto tra api e web (campo `priority`, nuovo endpoint); i due submodule vanno rilasciati insieme. Il campo in creazione è opzionale, quindi un web vecchio continua a funzionare con l'API nuova.

## Domande aperte
- [x] D1: Qual è la priorità di default quando non viene indicata in creazione? — risposta: `media` (proposta accettata nel thread); la priorità non è obbligatoria, `media` è anche preselezionata nel menu del web.
- [x] D2: L'ordine dell'elenco cambia in base alla priorità? — risposta: no, resta quello attuale (API per id; web prima i non fatti, poi per id).
- [x] D3: Forma dell'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con corpo `{ "priority": "alta" }`.
- [x] D4: Il web deve offrire anche filtro e modifica della priorità? — risposta: no, solo visualizzazione colorata e menu in creazione; filtro e modifica restano solo nell'API.
- [x] D5: Quali colori per le tre priorità? — risposta: bassa grigio-blu, media ambra, alta rosso, con il testo sempre visibile.
- [x] D6: Valori solo minuscoli? — risposta: sì, esattamente `bassa`, `media`, `alta`; maiuscole e varianti (`Alta`, `high`) → 400.
