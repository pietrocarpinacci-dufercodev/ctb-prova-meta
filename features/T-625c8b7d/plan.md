# T-625c8b7d — Piano

## Submodule coinvolti
- api: campo `priority` nel modello `Todo` e in `NewTodo`, validazione in creazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test e `README.md`.
- web: `priority` in `addTodo`, menu nel modulo di creazione, etichetta colorata nell'elenco, test. Niente filtro né modifica della priorità nel web (D6), `sortTodos` invariato (D2).

## Passi
1. api, `Program.cs`: valori ammessi in un solo punto, un array statico di stringhe `["bassa", "media", "alta"]` (D3) con default `"media"` (D1); `Todo` diventa `Todo(int Id, string Title, bool Done, string Priority)`; `NewTodo` riceve `string? Priority`.
2. api, `POST /api/todos`: se `priority` manca usa `media` (D1); se non è valido risponde `ValidationProblem` sul campo `priority`, come già fa per `title`.
3. api: `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`) ed endpoint `PUT /api/todos/{id:int}/priority` con body `{ "priority": "..." }`: 200 col todo, 404 se non esiste, 400 se il valore manca o non è valido.
4. api, `GET /api/todos`: parametro opzionale `string? priority`; se presente e valido filtra, se non valido 400, se assente tutto come oggi. Ordine della risposta invariato, per id (D2).
5. api, test xUnit in `TodoApiTests.cs`: creazione con priorità, default senza priorità, 400 su priorità non valida in creazione, `PUT` ok / 404 / 400, filtro ok / 400 / senza filtro. Aggiornare `api/README.md` con il campo e il nuovo endpoint. Verifica: `dotnet test api`.
6. web, `todos.js`: costanti `PRIORITIES = ["bassa", "media", "alta"]` e `DEFAULT_PRIORITY = "media"`; `addTodo(title, priority = DEFAULT_PRIORITY, fetchImpl = fetch)` valida la priorità e la invia nel body (attenzione: cambia la firma, aggiornare la chiamata in `app.js` e i test esistenti che passano `fakeFetch` come secondo argomento); `sortTodos` invariato (D2).
7. web, `index.html` + `app.js`: `<select id="priority">` con etichetta accessibile nel modulo, popolato da `PRIORITIES` e preselezionato su `media`; nell'elenco uno `<span class="priority priority-<valore>">` col testo della priorità (valore così com'è, D3) accanto al titolo.
8. web, `style.css`: bassa verde, media ambra, alta rosso (D7), con contrasto leggibile sul testo.
9. web, test in `tests/todos.test.js`: `addTodo` invia la priorità, rifiuta una priorità non valida senza chiamare l'API, usa `media` se omessa. Verifica: `npm test --prefix web`.
10. Il contratto cambia in entrambi i submodule (regola del `CLAUDE.md` del meta-repo): api e web vanno nella stessa feature e i test di entrambi devono passare.

## Fonti
- Richiesta nel thread Teams della feature T-625c8b7d e risposta alle domande aperte (proposte accettate, scelta più semplice dove mancava una proposta).
- `CLAUDE.md` del meta-repo: struttura, comandi, regola sul cambio di contratto e delega a `backend-dev` negli execute.
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda solo l'agente `backend-dev`; D4 "nessun agente Frontend" significa che anche il lavoro sul web passa da `backend-dev` o si fa senza agente).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: nessun servizio o dipendenza nuova; dati in memoria.
- Complessità: bassa, un campo enumerato, un endpoint, un filtro, un menu.
- Sicurezza: input validato lato API su un insieme chiuso di valori; il web inserisce la priorità con `textContent`, quindi nessun rischio di iniezione HTML. Nessuna autenticazione da toccare.
- Accoppiamento: cambia il contratto tra api e web (nuovo campo, firma di `addTodo`); i client che non inviano `priority` continuano a funzionare grazie al default `media` (D1).
- Rischio residuo: il contratto JSON usa valori in italiano (D3), diversi dalla lingua del codice; scelta voluta per semplicità.

## Domande aperte
- [x] D1: Qual è la priorità di default quando non è indicata in creazione (proposta: `media`), oppure il campo deve essere obbligatorio con 400 se manca? — risposta: default `media`, campo facoltativo (proposta accettata nel thread)
- [x] D2: Come va ordinato l'elenco: resta com'è (API per id; web prima i non fatti, poi per id) oppure, dentro i non fatti, prima per priorità (alta → bassa) e poi per id? E l'ordine va applicato nell'API, nel web o in entrambi? — risposta: l'ordine resta com'è in API e web (scelta più semplice, il thread ha delegato la scelta al bot)
- [x] D3: Nel JSON i valori restano in italiano (`bassa`, `media`, `alta`) come scritto nel thread, oppure in inglese (`low`, `medium`, `high`) con le etichette italiane solo nel web? — risposta: valori in italiano `bassa`/`media`/`alta` nel JSON, mostrati così come sono nel web, senza mappatura (scelta più semplice, il thread ha delegato la scelta al bot); gli identificatori del codice restano in inglese
- [x] D4: L'endpoint dedicato va bene come `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }`, o preferite un'altra forma? — risposta: `PUT /api/todos/{id}/priority` (proposta accettata)
- [x] D5: Il filtro `GET /api/todos?priority=<valore>` deve accettare un solo valore o più valori insieme, e con un valore non valido deve rispondere 400 o ignorare il filtro? — risposta: un solo valore, 400 se non valido (proposta accettata)
- [x] D6: Nel web servono anche il filtro per priorità e la modifica della priorità di un todo esistente? — risposta: no, nel web solo visualizzazione e scelta in creazione (proposta accettata)
- [x] D7: Ci sono colori già decisi per le tre priorità? — risposta: bassa verde, media ambra, alta rosso, con contrasto leggibile (proposta accettata; tra verde e grigio scelto verde)
