# T-a251369e — Piano

## Submodule coinvolti
- api: nuovo endpoint `DELETE /api/todos/{id}`, metodo `TodoStore.Remove`, test e README.
- web: nessuna modifica (la richiesta riguarda solo l'API; la UI non usa ancora la cancellazione).

## Passi
1. In `api/src/CtbProva.Api/Program.cs` aggiungere a `TodoStore` il metodo `bool Remove(int id)` basato su `_items.TryRemove`.
2. Mappare `app.MapDelete("/api/todos/{id:int}", ...)`: `204 No Content` se la cosa da fare esisteva ed è stata rimossa, `404 Not Found` altrimenti (stesso stile degli altri endpoint).
3. In `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` aggiungere i test:
   - `Deleted_todo_is_removed`: crea, DELETE → 204, GET successivo → 404, e non compare in `GET /api/todos`.
   - `Deleting_unknown_todo_returns_not_found`: DELETE su `/api/todos/9999` → 404.
   - `Deleting_twice_returns_not_found`: secondo DELETE sullo stesso id → 404.
4. Aggiornare la tabella degli endpoint in `api/README.md`.
5. Eseguire `dotnet test api`; commit nel submodule `api` sul branch `claude/T-a251369e` con messaggio `[T-a251369e] ...`, poi aggiornare il puntatore nel meta-repo.

## Domande aperte
- Codice di successo: propongo `204 No Content` (convenzione REST per DELETE). Se si preferisce `200` con la cosa rimossa nel corpo, dirlo nel thread.
