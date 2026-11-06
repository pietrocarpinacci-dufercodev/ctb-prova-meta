# T-e07d365f — Piano

## Submodule coinvolti
- api: endpoint `DELETE /api/todos/{id:int}` (204/404), `TodoStore.Remove`, test, riga nel README.
- web: `deleteTodo` in `src/todos.js`, pulsante "Elimina" in `src/app.js`, test, eventuale riga nel README.

## Passi
1. api, test prima (TDD) in `tests/CtbProva.Api.Tests/TodoApiTests.cs`: crea → DELETE 204 → GET 404 e assente dall'elenco; DELETE di id inesistente 404; secondo DELETE sullo stesso id 404.
2. api, `src/CtbProva.Api/Program.cs`: aggiungere `TodoStore.Remove(int id) => _items.TryRemove(id, out _)` e `app.MapDelete("/api/todos/{id:int}", (int id, TodoStore store) => store.Remove(id) ? Results.NoContent() : Results.NotFound());`. CORS già ammette ogni metodo.
3. api, `README.md`: aggiungere la riga DELETE alla tabella degli endpoint.
4. web, test prima in `tests/todos.test.js`: `deleteTodo` chiama `DELETE` su `/api/todos/{id}`, non legge il corpo con 204 e lancia `errore 404` se `!ok`.
5. web, `src/todos.js`: `export async function deleteTodo(id, fetchImpl = fetch)` con `method: "DELETE"`, `throw new Error(\`errore ${response.status}\`)` se `!response.ok`, nessun `json()`.
6. web, `src/app.js`: importare `deleteTodo`; in `render` aggiungere per ogni todo (fatto o no, D2) un pulsante "Elimina", dopo il pulsante "Fatto". Il click chiede `confirm(\`Eliminare "${todo.title}"?\`)` (D1); solo se confermato esegue `run(() => deleteTodo(todo.id))`, altrimenti non fa nulla. `confirm` sta solo in `app.js` (DOM), così `deleteTodo` resta testabile con `node --test`; la conferma non ha test automatici e si verifica a mano.
7. Verifica: `dotnet test api` e `npm test --prefix web`. Il contratto cambia in entrambi i submodule (regola del CLAUDE.md del meta-repo).
8. Descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`.

## Rischio
Livello: verde
- Costi: poche righe in due submodule, nessuna dipendenza nuova.
- Complessità: bassa; store in memoria su `ConcurrentDictionary`, `TryRemove` è atomico.
- Sicurezza: l'endpoint non ha autenticazione, come gli altri, con CORS aperto a ogni origine: chiunque raggiunga l'API può cancellare. Accettabile per un progetto di prova senza dati reali. La cancellazione è definitiva (dati solo in memoria).
- Accoppiamento: contratto API ↔ `web/src/todos.js`; i due submodule vanno rilasciati insieme (il web con "Elimina" contro un'API vecchia darebbe 405).

## Domande aperte
- [x] D1: il pulsante "Elimina" deve chiedere una conferma prima di cancellare? — risposta: sì, si chiede conferma (`confirm()` del browser); se l'utente annulla non si chiama l'API
- [x] D2: "Elimina" va mostrato anche sui todo già fatti? — risposta: sì, su ogni todo
