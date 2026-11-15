# T-7712db61 — Piano

## Submodule coinvolti
- api: nuova funzione statica `EvenNumbers.Sum` con doc comment e nuovi test xUnit; nessun endpoint nuovo.

## Passi
1. Nel submodule `api`, sul branch `claude/T-7712db61` (da `develop`), scrivere prima il test (TDD): nuovo file `api/tests/CtbProva.Api.Tests/EvenNumbersTests.cs` con un `[Theory]` xUnit sui casi della spec: `[1, 2, 3, 4]` → `6`, `[-2, 3, 4]` → `2`, `[]` → `0`, `[1, 3, 5]` → `0`, due `int.MaxValue - 1` (pari) → somma `long` senza overflow; più un `[Fact]` per `null` → `ArgumentNullException`.
2. Nuovo file `api/src/CtbProva.Api/EvenNumbers.cs`: `public static class EvenNumbers` con `public static long Sum(IEnumerable<int> numbers)`, implementata con LINQ (`ArgumentNullException.ThrowIfNull` + `numbers.Where(n => n % 2 == 0).Sum(n => (long)n)`), con doc comment `///` in inglese che spiega cosa fa. Classe pubblica nel namespace globale come gli altri tipi di `Program.cs`, così il progetto di test la vede senza modifiche al csproj.
3. Eseguire `dotnet test api` e verificare che passino i test nuovi e quelli esistenti.
4. Nessuna modifica a `Program.cs`, a `api/README.md` (nessun endpoint nuovo) né al submodule `web`.
5. Aggiornare `features/T-7712db61/decisions.md` e scrivere le descrizioni PR in `/work/out/meta.md` e `/work/out/api.md` (`[submodule "api"]` in `.gitmodules`).

## Fonti
- Richiesta nel thread Teams della feature (testo riportato in `spec.md`).
- `CLAUDE.md` del meta-repo: struttura, comando `dotnet test api`, regola sul contratto api/web.
- `.claude/agents/backend-dev.md`: convenzioni backend (test xUnit, codice minimo ma sempre con test).
- `features/T-9dfe1d36/decisions.md`: test xUnit confermati, nessun `api/CLAUDE.md`; nessuna contraddizione con questa feature.
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: due file nuovi e piccoli nel submodule api.
- Complessità: funzione pura di una riga con test unitari.
- Sicurezza: nessun input esterno, nessun endpoint, nessun dato persistito.
- Accoppiamento: nessun cambio al contratto HTTP, `web` non è toccato.

## Domande aperte
- [ ] D1: La funzione va esposta anche come endpoint HTTP (es. `POST /api/math/even-sum`) o basta la funzione C# con il test? Il piano assume solo la funzione: un endpoint cambierebbe il contratto e, per il CLAUDE.md, richiederebbe aggiornare `web/src/todos.js` e i test del web — fonti: richiesta del thread ("una funzione") vs `CLAUDE.md` del meta-repo (regola sul contratto api/web)
- [ ] D2: Tipo di ritorno `long` per evitare overflow sulle liste grandi, oppure `int` come gli elementi della lista? Il piano assume `long` — fonti: richiesta del thread (tipo non specificato) vs criterio di correttezza della spec
- [ ] D3: "Lista" va intesa come lista di interi (`IEnumerable<int>`)? Decimali o altri tipi numerici non hanno una parità definita e sono esclusi dal piano — fonti: richiesta del thread (tipo non specificato)
