# T-54c5d30f — Piano

## Submodule coinvolti
- ctb-prova-api (`api`): nuovo endpoint GET `/api/health/version`, test e riga nel README.

Il web non cambia: l'endpoint è additivo e il contratto esistente non viene toccato.

## Passi
1. Test in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` (sul modello del test di `/api/health`): GET `/api/health/version` risponde 200 e il JSON contiene `version` non vuota. Verificare che fallisca.
2. In `api/src/CtbProva.Api/Program.cs`, sotto `/api/health`, aggiungere `app.MapGet("/api/health/version", ...)` che restituisce `{ version }` letta da `Assembly.GetEntryAssembly()` (`AssemblyInformationalVersionAttribute`), senza nuove dipendenze.
3. Aggiungere la riga `GET /api/health/version` alla tabella di `api/README.md`.
4. Eseguire `dotnet test api`.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- Nessun allegato in `/work/inputs`; nessuna altra feature in `features/` con decisioni in conflitto.

## Rischio
Livello: verde
- Costi: minimi, una route e un test.
- Complessità: bassa, nessuna nuova dipendenza.
- Sicurezza: la versione è esposta senza autenticazione, come `/api/health`; CORS già aperto. Accettabile in un progetto di prova.
- Accoppiamento: solo l'API; contratto esistente invariato.

## Domande aperte
- [ ] D1: Quale deve essere la "versione dell'API"? Non esiste oggi alcuna versione definita nel progetto (nessun `<Version>` nel csproj, nessun tag): si propone la versione dell'assembly, con `<Version>` esplicito nel csproj (es. `1.0.0`) — fonti: richiesta del thread vs `api/src/CtbProva.Api/Program.cs` (nessuna versione presente)
- [ ] D2: Formato della risposta: va bene `{ "version": "1.0.0" }` oppure serve altro (es. commit, data di build)? — fonti: richiesta del thread vs `api/README.md` (contratto non specificato)
