# T-dd485a10 — Piano

## Submodule coinvolti
- api: campo `priority` nel record `Todo`, priorità opzionale in `NewTodo`, validazione, endpoint dedicato per cambiarla, filtro `?priority=` su `GET /api/todos`, test xUnit, `README.md`.
- web: `priority` in `addTodo` e nuova funzione per l'endpoint dedicato in `src/todos.js`, menu `<select>` nel form di `src/index.html`, etichetta colorata in `src/app.js` e `src/style.css`, test in `tests/todos.test.js`.

## Passi
1. API, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - creazione con priorità valida → 201 e `priority` restituita;
   - creazione senza priorità → default (D1);
   - creazione con priorità non valida → 400 con chiave `priority`;
   - endpoint dedicato (D4): 200 con priorità aggiornata, titolo e `done` invariati; 404 su id inesistente; 400 su valore non valido o assente;
   - `GET /api/todos?priority=<v>` restituisce solo quella priorità; valore non valido → 400 (D5);
   - ordine dell'elenco secondo D2.
2. API, implementazione in `api/src/CtbProva.Api/Program.cs`, mantenendo la struttura attuale (minimal API in un solo file, `TodoStore` in memoria):
   - `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`; un insieme statico dei valori ammessi (D3) usato da creazione, endpoint dedicato e filtro. Stringa validata invece di un enum: nessun converter JSON da configurare e i valori restano quelli del contratto;
   - confronto dei valori: esatto, minuscolo (proposta, vedi D3);
   - `TodoStore.Add(title, priority)` e `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`);
   - `GET /api/todos` con parametro opzionale `string? priority`;
   - messaggi di validazione in inglese per il codice nuovo (regola lingue); il messaggio esistente sul titolo resta com'è.
3. API: aggiornare la tabella endpoint di `api/README.md` (campo `priority`, endpoint dedicato, filtro).
4. Web, test prima in `web/tests/todos.test.js`:
   - `addTodo(title, priority, fetchImpl)` invia `priority` nel body (attenzione: cambia la firma; aggiornare il test esistente che passa `fakeFetch` come secondo argomento);
   - nuova funzione per l'endpoint dedicato (es. `setPriority(id, priority, fetchImpl)`) solo se serve al web (D6); altrimenti non si scrive;
   - `sortTodos` secondo D2;
   - eventuale funzione pura per la classe CSS/etichetta della priorità.
5. Web, implementazione:
   - `src/index.html`: `<select id="priority">` con le tre opzioni ed etichetta accessibile (`aria-label` o `<label>`), default D1;
   - `src/app.js`: legge il valore del menu in `addTodo`; in `render` aggiunge accanto al titolo uno `<span class="priority priority-<v>">` con il testo della priorità;
   - `src/style.css`: tre colori (proposta: bassa grigio/verde, media ambra, alta rosso) con contrasto sufficiente sul testo.
6. Meta-repo: aggiornare l'elenco endpoint nel `CLAUDE.md` del meta-repo con l'endpoint dedicato e il filtro (il contratto è condiviso tra `api` e `web`).
7. Verifica: `dotnet test api` e `npm test --prefix web`; lavoro sul codice delegato a `backend-dev` come da `CLAUDE.md`.
8. Descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md`; nuove decisioni in `decisions.md`.

Fuori ambito (non richiesto): persistenza, filtro per priorità nella pagina web, modifica della priorità dalla pagina web (salvo risposta diversa a D6), ordinamento configurabile.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, contratto API/web da riportare in `web/src/todos.js` e nei test di entrambi)
- `api/README.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`
- `features/T-9dfe1d36/decisions.md` (agente `backend-dev`, nessun agente frontend: D4)
- Regole del bot D45 (lingue: codice in inglese)
- Nessun allegato in `/work/inputs` usato

## Rischio
Livello: giallo
- Costi: nessun costo di runtime; due PR di submodule più quella del meta-repo.
- Complessità: bassa-media; un campo, un endpoint, un filtro e una modifica UI, ma con scelte di contratto ancora aperte (D1–D5).
- Sicurezza: nuovo input dal client (`priority` nel body e in query) da validare al confine contro l'insieme ammesso; nessun dato sensibile. In `app.js` il testo va inserito con `textContent`, come già avviene per il titolo.
- Accoppiamento: cambio di contratto tra `api` e `web` (campo nuovo, endpoint nuovo, firma di `addTodo`): le due PR vanno fuse insieme; il web vecchio continua a funzionare con l'API nuova solo se la priorità in creazione resta opzionale.

## Domande aperte
- [ ] D1: Qual è la priorità di default quando il client non la invia in creazione (e quale valore è preselezionato nel menu web)? Proposta: media — fonti: richiesta nel thread ("non abbiamo deciso la priorità di default")
- [ ] D2: Come va ordinato l'elenco? Opzioni: (a) come oggi (API per id, web prima i non fatti poi per id), (b) web: prima i non fatti, poi priorità alta→bassa, poi id; (c) lo stesso ordinamento anche nell'API — fonti: richiesta nel thread ("non abbiamo deciso l'ordine") vs `TodoStore.All()` e `sortTodos` attuali
- [ ] D3: Nel JSON i valori sono in italiano (`bassa`, `media`, `alta`, come scritto nel thread) o in inglese (`low`, `medium`, `high`, con le etichette italiane solo nella pagina web)? E il confronto è case-sensitive? Proposta: valori italiani minuscoli, confronto esatto — fonti: richiesta nel thread vs regola lingue del bot (codice in inglese)
- [ ] D4: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` oppure `POST /api/todos/{id}/priority` come l'esistente `/done`? Proposta: `PUT` con body — fonti: richiesta nel thread vs convenzione di `api/README.md` (`POST /api/todos/{id}/done`)
- [ ] D5: Il filtro accetta un solo valore (`?priority=alta`) o più valori (`?priority=alta&priority=media`)? Con un valore non ammesso risponde 400 o elenco vuoto? Proposta: un valore, 400 — fonti: richiesta nel thread (non specifica)
- [ ] D6: La pagina web deve permettere di cambiare la priorità di un todo esistente (che userebbe l'endpoint dedicato) e di filtrare per priorità, o solo mostrarla e sceglierla in creazione come scritto nel thread? Proposta: solo quanto scritto nel thread — fonti: richiesta nel thread vs `CLAUDE.md` del meta-repo (ogni cambio di contratto va riportato in `web/src/todos.js`)
