# T-c4945e41 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/count` in `src/CtbProva.Api/Program.cs`, un test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga nella tabella endpoint di `README.md`.
- web: nessuna modifica (D2: solo l'API; l'endpoint è aggiuntivo e non cambia il contratto esistente).

## Passi
1. Test prima (TDD): in `TodoApiTests.cs` aggiungi un test che crea un todo, legge `/api/todos/count` e verifica 200 e il campo `count`. Poiché lo store è condiviso nella fixture di classe, confronta il conteggio prima e dopo la creazione (+1) invece di un valore assoluto. Eseguilo e verifica che fallisca (404).
2. In `TodoStore` aggiungi `public int Count => _items.Count;` (su `ConcurrentDictionary`, thread-safe).
3. In `Program.cs` aggiungi `app.MapGet("/api/todos/count", (TodoStore store) => ...)` che restituisce `Results.Ok(new { count = store.Count })` (D1). Nessuna collisione con `/api/todos/{id:int}` grazie al vincolo `:int`.
4. Aggiorna la tabella endpoint in `api/README.md`.
5. Esegui `dotnet test api` e verifica che tutti i test passino.
6. Scrivi le descrizioni PR in `/work/out/api.md` e `/work/out/meta.md`; committa con `[T-c4945e41] ...`.

## Fonti
- `api/src/CtbProva.Api/Program.cs` (routing, `TodoStore`)
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` (stile dei test)
- `api/README.md` (tabella endpoint)
- `CLAUDE.md` del meta-repo (contratto API/web, regole sul piano)
- Nessun allegato in `/work/inputs` usato; nessuna `decisions.md` di altre feature presente.

## Rischio
Livello: verde
- Costi: minimi, circa 5 righe di codice e un test.
- Complessità: bassa; nessuna nuova dipendenza.
- Sicurezza: endpoint di sola lettura che espone solo un numero; CORS già aperto come per gli altri endpoint.
- Accoppiamento: endpoint additivo, il web non lo usa; il percorso `count` non collide con `{id:int}`.

## Domande aperte
- [x] D1: il corpo della risposta deve essere un numero nudo o un oggetto JSON? — risposta: oggetto `{ "count": 3 }`, come `/api/health`
- [x] D2: serve anche un uso lato web? — risposta: no, solo l'API
