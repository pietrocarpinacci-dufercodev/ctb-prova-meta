# T-2a7efd0f — Piano

## Submodule coinvolti
- api: nuova rotta `GET /api/todos/count` in `src/CtbProva.Api/Program.cs`, test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga in `README.md`.

## Passi
1. Test (TDD) in `TodoApiTests.cs`: count a 0 su store vuoto; count +1 dopo POST; GET `/api/todos/1` invariato.
2. In `Program.cs` aggiungere `app.MapGet("/api/todos/count", (TodoStore store) => Results.Ok(new { count = store.All().Count }));` (riuso di `All()`, nessun nuovo metodo nello store). La rotta `{id:int}` non confligge con `count`.
3. Aggiungere la riga nella tabella di `api/README.md`.
4. Eseguire `dotnet test api`.
5. Web: nessuna modifica (il web non usa il nuovo endpoint; non cambia alcun contratto esistente).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`.
- Nessun allegato in `/work/inputs`; nessuna decisione di altre feature.

## Rischio
Livello: verde
- Costi: minimi, una rotta e pochi test.
- Complessità: bassa, nessuna logica nuova.
- Sicurezza: endpoint in sola lettura, nessun dato sensibile.
- Accoppiamento: solo api; è un'aggiunta al contratto, non rompe i client esistenti.

## Domande aperte
- [ ] D1: formato della risposta: oggetto JSON `{ "count": n }` (default previsto, estendibile) oppure numero nudo `n`? — fonti: richiesta ("restituisce il numero") vs convenzione JSON degli altri endpoint
