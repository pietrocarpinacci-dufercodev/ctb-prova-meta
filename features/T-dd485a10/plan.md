# T-dd485a10 — Piano

## Submodule coinvolti
- api: campo `priority` nel record `Todo`, priorità opzionale in `NewTodo`, validazione, `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test xUnit, `README.md`.
- web: `priority` in `addTodo` (`src/todos.js`), menu `<select>` nel form di `src/index.html`, etichetta colorata in `src/app.js` e `src/style.css`, test in `tests/todos.test.js`.

## Passi
1. API, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - creazione con priorità ammessa → 201 e `priority` restituita;
   - creazione senza priorità → `media` (D1);
   - creazione con priorità non ammessa (es. `urgente`, `Alta`) → 400 con chiave `priority` (D3);
   - `PUT /api/todos/{id}/priority` (D4): 200 con priorità aggiornata, titolo e `done` invariati; 404 su id inesistente; 400 su valore non ammesso o assente;
   - `GET /api/todos?priority=alta` restituisce solo i todo `alta`; valore non ammesso → 400 (D5);
   - il test esistente `Created_todo_can_be_read_and_completed` continua a passare; il record `Todo` usato dai test prende il nuovo campo.
2. API, implementazione in `api/src/CtbProva.Api/Program.cs`, mantenendo la struttura attuale (minimal API in un solo file, `TodoStore` in memoria):
   - `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)` e `PriorityInput(string? Priority)` per il body del `PUT`;
   - un insieme statico dei valori ammessi `["bassa", "media", "alta"]` con confronto esatto (D3), usato da creazione, `PUT` e filtro; stringa validata invece di un enum (vedi `decisions.md`);
   - `TodoStore.Add(title, priority)` e `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone` (`with { Priority = ... }`);
   - `GET /api/todos` con parametro opzionale `string? priority`, un solo valore (D5); ordine invariato per id (D2);
   - messaggi di validazione nuovi in inglese (regola lingue); il messaggio esistente sul titolo resta com'è.
3. API: aggiornare la tabella endpoint di `api/README.md` (campo `priority` con default `media`, `PUT /api/todos/{id}/priority`, filtro `?priority=`).
4. Web, test prima in `web/tests/todos.test.js`:
   - `addTodo(title, priority, fetchImpl)` invia `priority` nel body; aggiornare i due test esistenti di `addTodo`, che oggi passano `fakeFetch` come secondo argomento;
   - `sortTodos` invariato (D2): il test esistente resta com'è.
5. Web, implementazione:
   - `src/todos.js`: `addTodo` accetta e invia `priority`; nessuna funzione per il `PUT` (D6);
   - `src/index.html`: `<select id="priority">` con `aria-label`, opzioni `bassa`/`media`/`alta`, `media` preselezionata;
   - `src/app.js`: passa il valore del menu ad `addTodo`; in `render` aggiunge accanto al titolo uno `<span class="priority priority-<v>">` con `textContent` uguale alla priorità; dopo la creazione riporta il menu su `media`;
   - `src/style.css`: `.priority-bassa` verde/grigio, `.priority-media` ambra, `.priority-alta` rosso, con contrasto sufficiente.
6. Meta-repo: aggiungere `PUT /api/todos/{id}/priority` e il filtro `?priority=` all'elenco endpoint del `CLAUDE.md` del meta-repo (contratto condiviso tra `api` e `web`).
7. Verifica: `dotnet test api` e `npm test --prefix web`; lavoro sul codice delegato a `backend-dev` come da `CLAUDE.md`.
8. Descrizioni PR in `/work/out/meta.md`, `/work/out/api.md`, `/work/out/web.md`; nuove decisioni in `decisions.md`.

Fuori ambito: persistenza, filtro e modifica della priorità nella pagina web (D6), ordinamento per priorità (D2), filtro su più valori (D5).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, contratto API/web da riportare in `web/src/todos.js` e nei test di entrambi)
- `api/README.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`
- `features/T-9dfe1d36/decisions.md` (agente `backend-dev`, nessun agente frontend: D4)
- Regole del bot D45 (lingue: codice in inglese)
- Risposte del thread: proposte accettate per tutte le domande aperte
- Nessun allegato in `/work/inputs` usato

## Rischio
Livello: giallo
- Costi: nessun costo di runtime; due PR di submodule più quella del meta-repo.
- Complessità: bassa; un campo, un endpoint, un filtro e una modifica UI, con il contratto ora fissato.
- Sicurezza: nuovo input dal client (`priority` nel body e in query) validato al confine contro l'insieme ammesso; nessun dato sensibile. In `app.js` la priorità va inserita con `textContent` e la classe CSS costruita solo da valori ammessi.
- Accoppiamento: cambio di contratto tra `api` e `web` (campo nuovo, endpoint nuovo, firma di `addTodo`): le due PR vanno fuse insieme; il web attuale continua a funzionare con l'API nuova perché la priorità in creazione è opzionale.

## Domande aperte
- [x] D1: Qual è la priorità di default quando il client non la invia in creazione (e quale valore è preselezionato nel menu web)? — risposta: `media` (proposta accettata nel thread)
- [x] D2: Come va ordinato l'elenco? — risposta: come oggi (API per id, web prima i non fatti poi per id); la priorità non cambia l'ordine. Il thread ha lasciato la scelta al bot: è l'opzione più semplice
- [x] D3: Valori nel JSON in italiano o in inglese, e confronto case-sensitive? — risposta: `bassa`, `media`, `alta` minuscoli, confronto esatto (proposta accettata nel thread); identificatori del codice in inglese
- [x] D4: Forma dell'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` (proposta accettata nel thread)
- [x] D5: Filtro su uno o più valori, e risposta con valore non ammesso? — risposta: un solo valore, 400 con valore non ammesso (proposta accettata nel thread)
- [x] D6: La pagina web deve anche cambiare la priorità e filtrare? — risposta: no, solo mostrarla e sceglierla in creazione (proposta accettata nel thread)
