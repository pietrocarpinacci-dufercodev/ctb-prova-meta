# T-fdec83a5 — Piano

## Submodule coinvolti
- api: `GET /api/health` esiste già (`api/src/CtbProva.Api/Program.cs:10`, risponde `{ "status": "ok" }`, test `Health_returns_ok`). Si estende solo se D1 lo richiede (campi extra) e si aggiorna `api/README.md` e il test.

## Passi
1. Attendere la risposta a D1 (l'endpoint esiste già: cosa va aggiunto?).
2. Se D1 = "niente da fare": chiudere l'execute senza modifiche, segnalandolo.
3. Se D1 = campi extra (es. `timestamp` UTC, `version`): con `backend-dev`, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, poi modifica minima della MapGet in `Program.cs`; aggiornare la tabella in `api/README.md`.
4. `dotnet test api`.
5. Nessuna modifica a `web`; il contratto `status` resta invariato, quindi `web/src/todos.js` non cambia.
6. Nell'esito dell'execute indicare il subagent usato (atteso `backend-dev`, come da CLAUDE.md) e scrivere le PR in `/work/out`.

## Fonti
- `CLAUDE.md` del meta-repo (contratto API, comandi, regola `backend-dev`)
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`
- `features/T-9dfe1d36/decisions.md` (agente `backend-dev`)
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: minimi, poche righe in un solo submodule.
- Complessità: bassa; endpoint già presente.
- Sicurezza: endpoint pubblico; non esporre dettagli interni (versioni, percorsi, stack) senza necessità.
- Accoppiamento: il web usa `/api/health`; aggiungere soli campi, mai rimuovere `status`.

## Domande aperte
- [ ] D1: `GET /api/health` esiste già e risponde `{ "status": "ok" }`: cosa deve cambiare? (nulla / aggiungere `timestamp`, `version`, `uptime` / controllo dipendenze) — fonti: richiesta del thread vs `api/src/CtbProva.Api/Program.cs`
