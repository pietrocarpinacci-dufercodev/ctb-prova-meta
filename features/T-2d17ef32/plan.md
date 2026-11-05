# T-2d17ef32 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/count` con test.
- web: nessuna modifica (il web non usa il nuovo endpoint e il contratto esistente non cambia).

## Passi
1. In `api/src/CtbProva.Api/Program.cs` aggiungere `app.MapGet("/api/todos/count", (TodoStore store) => Results.Ok(new { count = store.Count() }));` e il metodo `TodoStore.Count()` (`_items.Count`). Il percorso `count` è statico e non confligge con `/api/todos/{id:int}` (vincolo `int`).
2. In `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` aggiungere un test: leggere il conteggio iniziale, creare due cose da fare via POST, verificare che `GET /api/todos/count` risponda 200 con `count` aumentato di 2. Il confronto è relativo perché la factory è condivisa tra i test della classe.
3. Aggiornare la tabella degli endpoint in `api/README.md` con la riga `GET /api/todos/count`.
4. Eseguire `dotnet test api`, poi committare nel submodule `api` sul branch `claude/T-2d17ef32` con messaggio `[T-2d17ef32] <cosa>`.

## Domande aperte
- Formato della risposta: propongo l'oggetto JSON `{ "count": N }` (coerente con `/api/health`). Se si preferisce un numero nudo, cambia il passo 1.
- Il conteggio include anche le cose già fatte? Propongo di sì (totale).
