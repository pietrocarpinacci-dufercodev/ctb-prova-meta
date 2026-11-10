# T-fdec83a5 — Piano

## Submodule coinvolti
- api: `GET /api/health` (`api/src/CtbProva.Api/Program.cs:10`) oggi risponde `{ "status": "ok" }`; si aggiunge il campo `dotnetVersion` con la versione di .NET in uso, si aggiornano il test e `api/README.md`.

## Passi
1. Con `backend-dev` e TDD: nel test `Health_returns_ok` (`api/tests/CtbProva.Api.Tests/TodoApiTests.cs`) leggere il JSON e verificare `status` = `ok` e `dotnetVersion` uguale a `Environment.Version.ToString()` (non vuoto).
2. Modificare la MapGet in `Program.cs`: `Results.Ok(new { status = "ok", dotnetVersion = Environment.Version.ToString() })`.
3. Aggiornare la riga di `/api/health` in `api/README.md` con il nuovo campo.
4. `dotnet test api`.
5. Nessuna modifica a `web`: `status` resta invariato, il campo extra è additivo.
6. Nell'esito dell'execute indicare il subagent usato (atteso `backend-dev`) e scrivere le PR in `/work/out`.

## Fonti
- `CLAUDE.md` del meta-repo (contratto API, comandi, regola `backend-dev`)
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`
- `features/T-9dfe1d36/decisions.md` (agente `backend-dev`)
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: minimi, poche righe in un solo submodule.
- Complessità: bassa.
- Sicurezza: l'endpoint è pubblico e la versione del runtime è un'informazione di fingerprinting; richiesta esplicitamente, si espone solo la versione di .NET e nient'altro.
- Accoppiamento: il web usa `/api/health`; il campo è additivo, `status` non cambia.

## Domande aperte
- [x] D1: cosa deve cambiare nell'health? — risposta: aggiungere la versione di .NET in uso (thread); nome campo `dotnetVersion` e valore `Environment.Version` scelti dal piano, modificabili in review.
