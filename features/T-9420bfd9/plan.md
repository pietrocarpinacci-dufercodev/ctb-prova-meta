# T-9420bfd9 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo` e `NewTodo` con validazione e default `media`, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test xUnit, `README.md`
- web: menu della priorità nel form (preselezionato su `media`), priorità colorata accanto a ogni todo, `addTodo` che manda la priorità, `sortTodos` per stato, poi priorità, poi id, test `node --test`

## Passi
1. **api, test prima (TDD)**: in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` scrivere i test per:
   - creazione con `priority` valida;
   - creazione senza `priority`, che usa `media`;
   - priorità non valida o con maiuscole (`"Alta"`), che risponde 400;
   - `PUT /api/todos/{id}/priority`, con le risposte 200, 400 e 404;
   - filtro `GET /api/todos?priority=alta`;
   - filtro non valido, che risponde 400;
   - elenco ancora ordinato per id.

   Aggiornare il record `Todo` usato dai test.
2. **api, codice** in `api/src/CtbProva.Api/Program.cs`:
   - `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)` e il body della modifica `{ "priority": "..." }`;
   - un solo insieme dei valori ammessi (`bassa`, `media`, `alta`, minuscoli, confronto esatto), usato per validare creazione, modifica e filtro; le risposte 400 usano `Results.ValidationProblem` sul campo `priority`, come per il titolo;
   - `TodoStore.Add(title, priority)` e `TodoStore.SetPriority(id, priority)`, sullo stesso modello di `MarkDone`;
   - `MapPut("/api/todos/{id:int}/priority", ...)`, che risponde 200 con il todo aggiornato;
   - `GET /api/todos` con `string? priority` opzionale da query string;
   - `All()` resta ordinato per id;
   - i valori restano stringhe, senza enum né converter.
3. **api, documentazione**: aggiornare la tabella degli endpoint in `api/README.md` con il nuovo endpoint, il campo `priority` in creazione con default `media` e il filtro. Eseguire `dotnet test api`.
4. **web, test prima** in `web/tests/todos.test.js`:
   - `addTodo` manda `priority` nel body;
   - `sortTodos` mette prima i non fatti, poi la priorità da alta a bassa, poi l'id;
   - un todo senza priorità o con una sconosciuta non manda in errore l'ordinamento e va dopo `bassa`.
5. **web, codice**:
   - `web/src/todos.js`:
     - costante `PRIORITIES = ["bassa", "media", "alta"]`;
     - `addTodo(title, priority, fetchImpl)` manda `{ title, priority }`;
     - `sortTodos` usa le chiavi dello step 4;
   - `web/src/index.html`: `<label>` e `<select id="priority">` con le tre opzioni, `media` selezionata;
   - `web/src/app.js`:
     - legge la select al submit;
     - in `render` aggiunge `<span class="priority priority-<valore>">` con il testo della priorità tramite `textContent`;
     - non aggiunge una classe per i valori sconosciuti;
   - `web/src/style.css`: colori per `.priority-bassa` (verde), `.priority-media` (ambra) e `.priority-alta` (rosso), con contrasto almeno 4.5:1 sul fondo `#f6f7f9`;
   - filtro e modifica della priorità dal web restano fuori ambito (D6);
   - eseguire `npm test --prefix web`.
6. **meta-repo**:
   - aggiornare l'elenco degli endpoint in `CLAUDE.md`, aggiungendo `/api/todos/{id}/priority` e `?priority=`;
   - aggiornare `decisions.md`;
   - committare i puntatori di `api` e `web`;
   - scrivere le descrizioni PR in `/work/out/meta.md`, `/work/out/api.md` e `/work/out/web.md`.
7. **Agente (D7)**: delegare i passi 1–3 a `backend-dev` e fare i passi 4–5 senza agente, scrivendolo nell'esito.
8. **Verifica manuale (fuori dal container, se possibile)**: avviare API e web, creare todo con le tre priorità, controllare colori e ordine.

## Fonti
- Richieste nel thread Teams della feature T-9420bfd9: la prima richiesta e la risposta "va bene la tua proposta" per tutte le domande aperte. Nessun allegato in `/work/inputs`.
- `CLAUDE.md` del meta-repo: struttura, comandi, regola sul contratto API ↔ web e regola su `backend-dev`.
- `.claude/agents/backend-dev.md`
- `features/T-9dfe1d36/decisions.md` (D4: nessun agente Frontend)
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`

## Rischio
Livello: verde
- Costi: lavoro piccolo su due submodule, senza dipendenze nuove né infrastruttura.
- Complessità: un campo, un endpoint e un filtro in una minimal API; select e badge nel web. I dati sono in memoria, quindi niente migrazioni.
- Sicurezza: c'è un nuovo input, nel body e nella query string, da validare contro un insieme chiuso di valori. Il web inserisce il testo con `textContent`, quindi non può iniettare HTML.
- Accoppiamento: cambia il contratto tra `api` e `web`, con un nuovo campo nella risposta e nella creazione. Il campo è facoltativo in creazione, quindi i client esistenti continuano a funzionare. Le due PR vanno comunque rilasciate insieme.

## Domande aperte
- [x] D1: Qual è la priorità di default quando la creazione non la indica (API) e quale opzione è preselezionata nel menu del web? — risposta: `media` in entrambi
- [x] D2: In che ordine va restituito e mostrato l'elenco? — risposta: l'API resta ordinata per id; il web mostra prima i non fatti, poi la priorità da alta a bassa, poi l'id
- [x] D3: Che formato ha la priorità nel JSON, e il confronto distingue le maiuscole? — risposta: stringhe minuscole `"bassa"`/`"media"`/`"alta"`, confronto esatto (le maiuscole danno 400)
- [x] D4: Quale forma per l'endpoint dedicato? — risposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }`, risposta 200 con il todo
- [x] D5: Un filtro con valore non valido risponde 400 o con un elenco vuoto? — risposta: 400
- [x] D6: Il web deve usare anche il filtro e la modifica della priorità? — risposta: no, fuori ambito
- [x] D7: Nell'execute la parte web va delegata a `backend-dev`? — risposta: `backend-dev` per `api`, web senza agente, scritto nell'esito
- [x] D8: Quali colori per le tre priorità? — risposta: bassa verde, media ambra, alta rosso, con il testo sempre visibile e contrasto almeno 4.5:1
