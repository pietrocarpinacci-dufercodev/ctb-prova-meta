# T-d1dfa20c — Piano

Spec: `features/T-d1dfa20c/spec.md`. Decisioni: `features/T-d1dfa20c/decisions.md`.

## Submodule coinvolti
- api: aggiunge `Priority` al modello `Todo`, validazione, `PATCH /api/todos/{id}`, test, `api/README.md`.
- web: selettore di priorità nel form e per ogni voce, funzione `setPriority` in `src/todos.js`, test, `web/README.md` se elenca il contratto.
- meta-repo: aggiornare l'elenco endpoint in `CLAUDE.md` (contratto API) — solo in execute.

## Passi
1. api (test prima): in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` aggiungere test per creazione con/senza priorità, priorità non valida in POST e in PATCH (400), PATCH senza `priority` (400), PATCH ok (200) e id inesistente (404), campo `priority` in tutte le risposte.
2. api: in `Program.cs` cambiare `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`; insieme ammesso `high`/`medium`/`low`; `TodoStore.Add` accetta la priorità; nuovo `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone`.
3. api: validare `priority` in `POST /api/todos` (`ValidationProblem` sul campo `priority`); nuovo `MapPatch("/api/todos/{id:int}")` con record `PatchTodo(string? Priority)`: 400 se manca o non valida, 404 se l'id non esiste. CORS già con `AllowAnyMethod`, nessuna modifica.
4. api: aggiornare la tabella endpoint in `api/README.md`. `dotnet test api`.
5. web (test prima): in `web/tests/todos.test.js` testare `setPriority` (metodo PATCH, URL, corpo, errore) e `addTodo` con priorità.
6. web: `todos.js` — `PRIORITIES`, `setPriority(id, priority, fetchImpl)`, `addTodo(title, priority, fetchImpl)` che invia `priority`.
7. web: `index.html` — `<select id="priority">` nel form; `app.js` — per ogni voce un `<select>` con la priorità corrente che chiama `setPriority` via `run`; `style.css` — distinzione visiva minima per priorità (testo oltre al colore, per accessibilità). `npm test --prefix web`.
8. Aggiornare l'elenco endpoint in `CLAUDE.md` e riportare il contratto nei test di entrambi i submodule (regola del meta-repo).
9. Descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`.

## Rischio
Livello: verde
- Costi: basso, due submodule piccoli, nessuna dipendenza nuova.
- Complessità: bassa; un campo, la validazione e un PATCH sul modello di `done`.
- Sicurezza: bassa; il valore è validato lato API contro un insieme chiuso, il web usa `textContent` e non `innerHTML`.
- Accoppiamento: medio-basso; cambia il contratto API (campo `priority` e nuovo verbo PATCH) e va rilasciato insieme in api e web.

## Domande aperte
- [x] D1: Valori della priorità nel contratto — risposta: in inglese, `high`, `medium`, `low`.
- [x] D2: Come si modifica la priorità — risposta: `priority` sia in `POST /api/todos` sia in `PATCH /api/todos/{id}`.
- [ ] D3: Qual è il valore predefinito per le voci create senza priorità (assunto `medium`)? — fonti: richiesta (silente) vs spec (criterio con `medium`)
- [ ] D4: Il PATCH accetta solo `priority` (assunto) o anche `title` e `done`? Il CLAUDE.md e il README descrivono `done` come azione dedicata `POST /done` — fonti: risposta D2 ("PATCH todo") vs CLAUDE.md e `api/README.md`
- [ ] D5: Va cambiato l'ordinamento dell'elenco (oggi: non fatte prima, poi per id in `sortTodos`)? Ad esempio `high`→`low` tra le non fatte — fonti: richiesta ("visibile e modificabile") vs `web/src/todos.js` (`sortTodos`) e relativi test
- [ ] D6: Si può cambiare la priorità di una voce già fatta? Assunto sì, senza restrizioni — fonti: richiesta (silente) vs `app.js` (le voci fatte non hanno azioni)
