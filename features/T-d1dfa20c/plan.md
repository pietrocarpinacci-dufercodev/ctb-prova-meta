# T-d1dfa20c — Piano

Spec: `features/T-d1dfa20c/spec.md`.

## Submodule coinvolti
- api: aggiunge `Priority` al modello `Todo`, validazione, nuovo endpoint `POST /api/todos/{id}/priority`, test, `api/README.md`.
- web: selettore di priorità nel form e per ogni voce, funzione `setPriority` in `src/todos.js`, test, `web/README.md` se elenca il contratto.
- meta-repo: aggiornare l'elenco endpoint in `CLAUDE.md` (contratto API) — solo in execute.

## Passi
1. api (test prima): in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` aggiungere test per creazione con/senza priorità, priorità non valida (400), cambio priorità (200 e 404), campo `priority` nelle risposte.
2. api: in `Program.cs` cambiare `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`; costante con i tre valori ammessi; `TodoStore.Add` accetta la priorità; nuovo `TodoStore.SetPriority(id, priority)` sul modello di `MarkDone`.
3. api: endpoint `POST /api/todos/{id:int}/priority` (corpo `{ "priority": "..." }`), 400 con `ValidationProblem` se il valore non è ammesso, 404 se l'id manca. Validare anche `priority` in `POST /api/todos`.
4. api: aggiornare la tabella endpoint in `api/README.md`. `dotnet test api`.
5. web (test prima): in `web/tests/todos.test.js` testare `setPriority` (URL, corpo, errore) e `addTodo` con priorità.
6. web: `todos.js` — `PRIORITIES`, `setPriority(id, priority, fetchImpl)`, `addTodo(title, priority, fetchImpl)` che invia `priority`.
7. web: `index.html` — `<select id="priority">` nel form; `app.js` — per ogni voce un `<select>` con la priorità corrente che chiama `setPriority` via `run`; `style.css` — distinzione visiva minima per priorità (testo, non solo colore, per accessibilità). `npm test --prefix web`.
8. Aggiornare l'elenco endpoint in `CLAUDE.md` e riportare nei test di entrambi i submodule il contratto (regola del meta-repo).
9. Descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`.

## Rischio
Livello: verde
- Costi: basso, due submodule piccoli, nessuna dipendenza nuova.
- Complessità: bassa; un campo e un endpoint sul modello di `done`.
- Sicurezza: bassa; il valore è validato lato API contro un insieme chiuso, il web usa `textContent` e non `innerHTML`.
- Accoppiamento: medio-basso; cambia il contratto API (campo `priority` e nuovo endpoint) e va rilasciato insieme in api e web. Il web resta compatibile con risposte senza `priority` solo se si gestisce il valore mancante (da decidere in D1).

## Domande aperte
- [ ] D1: I valori della priorità nel contratto sono `alta`/`media`/`bassa` (italiano, come nella richiesta) o `high`/`medium`/`low`? E quale è il valore predefinito per le voci create senza priorità (assunto `media`)? — fonti: richiesta nel thread vs contratto API esistente (campi in inglese: `title`, `done`)
- [ ] D2: Come si modifica la priorità: nuovo `POST /api/todos/{id}/priority` (assunto, coerente con `/done`) oppure `PATCH /api/todos/{id}`? Il CLAUDE.md elenca solo gli endpoint esistenti e l'API non ha ancora aggiornamenti parziali — fonti: richiesta ("modificabile") vs CLAUDE.md e `api/README.md`
- [ ] D3: Va cambiato l'ordinamento dell'elenco (oggi: non fatte prima, poi per id in `sortTodos`)? Ad esempio alta→bassa tra le non fatte. La richiesta dice solo "visibile e modificabile" — fonti: richiesta vs `web/src/todos.js` (`sortTodos`) e relativi test
- [ ] D4: Si può cambiare la priorità di una voce già fatta? Assunto sì, senza restrizioni — fonti: richiesta (silente) vs `app.js` (le voci fatte non hanno azioni)
