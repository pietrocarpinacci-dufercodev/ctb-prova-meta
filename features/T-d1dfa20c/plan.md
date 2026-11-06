# T-d1dfa20c — Piano

Spec: `features/T-d1dfa20c/spec.md`. Decisioni: `features/T-d1dfa20c/decisions.md`.

## Submodule coinvolti
- api: `Priority` nel modello `Todo`, obbligatoria in POST, `PATCH /api/todos/{id}` parziale su `title` e `priority`, test, `api/README.md`.
- web: selettore di priorità nel form (senza valore preselezionato) e per ogni voce (anche fatte), `setPriority` in `src/todos.js`, ordinamento per priorità in `sortTodos`, test, `web/README.md` se elenca il contratto.
- meta-repo: aggiornare l'elenco endpoint in `CLAUDE.md` (contratto API) — solo in execute.

## Passi
1. api (test prima): in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` aggiornare i test esistenti del POST (ora serve `priority`) e aggiungere: POST senza priorità o con valore non valido (400, nulla creato); PATCH con solo `priority`, con solo `title`, con entrambi, con corpo vuoto (nessuna modifica); PATCH con valore non valido (400); PATCH su voce fatta (200); id inesistente (404); campo `priority` in tutte le risposte.
2. api: in `Program.cs` cambiare `Todo(int Id, string Title, bool Done, string Priority)`; `NewTodo(string? Title, string? Priority)`; insieme ammesso `high`/`medium`/`low`; `TodoStore.Add` accetta la priorità; nuovo `TodoStore.Update(id, title, priority)` che applica solo i campi presenti.
3. api: `POST /api/todos` valida `title` e `priority` (`ValidationProblem` con i campi in errore). `MapPatch("/api/todos/{id:int}")`: distingue campo assente da `null` leggendo il corpo come `JsonElement`/`JsonObject` (con un record normale i due casi sono indistinguibili); campi presenti validati, `null` accettato solo se il campo lo prevede; 404 se l'id manca. CORS già con `AllowAnyMethod`.
4. api: aggiornare la tabella endpoint in `api/README.md`. `dotnet test api`.
5. web (test prima): in `web/tests/todos.test.js` testare `setPriority` (metodo PATCH, URL, corpo con solo `priority`, errore), `addTodo(title, priority)` che invia la priorità e rifiuta se manca, e `sortTodos` (non fatte prima, poi `high`→`low`, poi id).
6. web: `todos.js` — `PRIORITIES`, `setPriority(id, priority, fetchImpl)`, `addTodo(title, priority, fetchImpl)`, `sortTodos` con ordine per priorità.
7. web: `index.html` — `<select id="priority" required>` con opzione vuota iniziale; `app.js` — per ogni voce (anche fatta) un `<select>` con la priorità corrente che chiama `setPriority` via `run`; `style.css` — distinzione visiva minima per priorità (testo oltre al colore). `npm test --prefix web`.
8. Aggiornare l'elenco endpoint in `CLAUDE.md` e riportare il contratto nei test di entrambi i submodule (regola del meta-repo).
9. Descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`.

## Rischio
Livello: giallo
- Costi: basso, due submodule piccoli, nessuna dipendenza nuova.
- Complessità: media-bassa; il PATCH parziale con distinzione assente/`null` richiede di leggere il JSON grezzo invece di un record, e va testato bene.
- Sicurezza: bassa; valori validati lato API contro un insieme chiuso, il web usa `textContent` e non `innerHTML`.
- Accoppiamento: medio; `priority` obbligatoria in POST è un cambio non compatibile del contratto: un client che invia solo `title` riceve 400. Api e web vanno rilasciati insieme (il web esistente si rompe con la sola api nuova).

## Domande aperte
- [x] D1: Valori della priorità — risposta: `high`, `medium`, `low`.
- [x] D2: Come si modifica la priorità — risposta: `priority` in `POST /api/todos` e `PATCH /api/todos/{id}`.
- [x] D3: Valore predefinito — risposta: nessun predefinito, `priority` obbligatoria in POST.
- [x] D4: Campi del PATCH — risposta: gli stessi del POST; assente = invariato, presente = aggiornato, `null` = messo a null se la proprietà lo prevede. Per il POST attuale i campi sono `title` e `priority`; `done` resta con `POST /done`.
- [x] D5: Ordinamento — risposta: sì, per priorità.
- [x] D6: Priorità di una voce fatta — risposta: sì, con il PATCH.
- [ ] D7: Nessun campo ammette `null` (`title` obbligatorio, `priority` obbligatoria in POST per D3): quindi un PATCH con `"priority": null` o `"title": null` risponde 400. È corretto, o `priority` deve poter tornare a `null` (e allora in POST è obbligatoria ma opzionale dopo, e il web come la mostra e dove la ordina)? — fonti: risposta D4 ("va messo a null se la proprietà lo prevede") vs risposta D3 (priorità obbligatoria alla creazione)
- [ ] D8: Come si combina l'ordinamento con lo stato `done`? Assunto: non fatte prima, poi `high`→`medium`→`low`, poi id, applicato nel web in `sortTodos` (l'API continua a restituire per id). Va bene, o l'ordine per priorità vale prima di `done`, o va fatto nell'API? — fonti: risposta D5 vs `web/src/todos.js` (`sortTodos`: non fatte prima) e relativi test
