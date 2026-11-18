# T-625c8b7d — Piano

## Submodule coinvolti
- api: campo `priority` nel modello `Todo` e in `NewTodo`, validazione in creazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test e `README.md`.
- web: `priority` in `addTodo`, nuova funzione `setPriority` solo se D6 lo chiede, menu nel modulo di creazione, etichetta colorata nell'elenco, eventuale ordinamento per priorità in `sortTodos` (D2), test.

## Passi
1. api, `Program.cs`: valori ammessi in un solo punto (array statico di stringhe `["bassa", "media", "alta"]` o enum serializzato come stringa, secondo D3); `Todo` diventa `Todo(int Id, string Title, bool Done, string Priority)`; `NewTodo` riceve `string? Priority`.
2. api, `POST /api/todos`: se `priority` manca usa il default (D1); se non è valido risponde `ValidationProblem` sul campo `priority`, come già fa per `title`.
3. api: `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`) ed endpoint `PUT /api/todos/{id:int}/priority` con body `{ "priority": "..." }`: 200 col todo, 404 se non esiste, 400 se il valore manca o non è valido.
4. api, `GET /api/todos`: parametro opzionale `string? priority`; se presente e valido filtra, se non valido 400, se assente tutto come oggi. Ordine della risposta secondo D2 (oggi per id).
5. api, test xUnit in `TodoApiTests.cs`: creazione con priorità, default senza priorità, 400 su priorità non valida in creazione, `PUT` ok / 404 / 400, filtro ok / 400 / senza filtro. Aggiornare `api/README.md` con il campo e il nuovo endpoint. Verifica: `dotnet test api`.
6. web, `todos.js`: costante `PRIORITIES` con i tre valori e il default; `addTodo(title, priority, fetchImpl)` valida la priorità e la invia nel body (attenzione: cambia la firma, aggiornare la chiamata in `app.js` e i test esistenti che passano `fakeFetch` come secondo argomento); `sortTodos` aggiornato solo se D2 lo chiede.
7. web, `index.html` + `app.js`: `<select id="priority">` con etichetta accessibile nel modulo, popolato da `PRIORITIES` e preselezionato sul default; nell'elenco uno `<span class="priority priority-<valore>">` col testo della priorità accanto al titolo.
8. web, `style.css`: tre colori (proposta: bassa grigio/verde, media ambra, alta rosso) con contrasto leggibile sul testo.
9. web, test in `tests/todos.test.js`: `addTodo` invia la priorità, rifiuta una priorità non valida senza chiamare l'API, usa il default se omessa; test di `sortTodos` se cambia l'ordine. Verifica: `npm test --prefix web`.
10. Il contratto cambia in entrambi i submodule (regola del `CLAUDE.md` del meta-repo): api e web vanno nella stessa feature e i test di entrambi devono passare.

## Fonti
- Richiesta nel thread Teams della feature T-625c8b7d.
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
- Accoppiamento: cambia il contratto tra api e web (nuovo campo, firma di `addTodo`); i client che non inviano `priority` continuano a funzionare grazie al default, se D1 non lo rende obbligatorio.

## Domande aperte
- [ ] D1: Qual è la priorità di default quando non è indicata in creazione (proposta: `media`), oppure il campo deve essere obbligatorio con 400 se manca? — fonti: richiesta nel thread ("non abbiamo deciso quale debba essere la priorità di default") vs criterio di creazione in spec.md
- [ ] D2: Come va ordinato l'elenco: resta com'è (API per id; web prima i non fatti, poi per id) oppure, dentro i non fatti, prima per priorità (alta → bassa) e poi per id? E l'ordine va applicato nell'API, nel web o in entrambi? — fonti: richiesta nel thread ("non abbiamo deciso l'ordine dell'elenco") vs `TodoStore.All()` e `sortTodos` attuali
- [ ] D3: Nel JSON i valori restano in italiano (`bassa`, `media`, `alta`) come scritto nel thread, oppure in inglese (`low`, `medium`, `high`) con le etichette italiane solo nel web? — fonti: richiesta nel thread vs regola di lingua del progetto (codice e testi nel codice in inglese)
- [ ] D4: L'endpoint dedicato va bene come `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }`, o preferite un'altra forma (per esempio `POST /api/todos/{id}/priority` come `/done`, o `PATCH /api/todos/{id}`)? — fonti: richiesta nel thread ("endpoint dedicato") vs stile degli endpoint esistenti in `api/README.md`
- [ ] D5: Il filtro `GET /api/todos?priority=<valore>` deve accettare un solo valore (proposta) o più valori insieme, e con un valore non valido deve rispondere 400 (proposta) o ignorare il filtro? — fonti: richiesta nel thread ("filtrare l'elenco per priorità") vs contratto attuale di `GET /api/todos` senza parametri
- [ ] D6: Nel web servono anche il filtro per priorità e la modifica della priorità di un todo esistente, visto che l'API li offre, o solo visualizzazione e scelta in creazione come chiesto? — fonti: richiesta nel thread (parte web) vs parte API della stessa richiesta
- [ ] D7: Ci sono colori già decisi per le tre priorità (proposta: bassa verde/grigio, media ambra, alta rosso)? — fonti: richiesta nel thread ("un colore diverso") vs `web/src/style.css` senza palette definita
