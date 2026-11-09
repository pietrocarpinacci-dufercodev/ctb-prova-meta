# T-e1ade1c1 — Piano

## Submodule coinvolti
- api: nuovo endpoint `DELETE /api/todos/completed` in `src/CtbProva.Api/Program.cs`, metodo `TodoStore.RemoveCompleted()`, test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga nuova in `README.md`.

## Passi
1. (TDD) In `TodoApiTests.cs` aggiungere test: crea 2 todo, segna il primo fatto, `DELETE /api/todos/completed` → 204; il primo dà 404, il secondo resta. Altro test: nessun completato → 204 e lista invariata.
2. In `Program.cs` aggiungere `TodoStore.RemoveCompleted()` (itera `_items`, `TryRemove` dei todo con `Done`; thread-safe come il resto dello store).
3. Mappare `app.MapDelete("/api/todos/completed", ...)` → `Results.NoContent()`. Nessun conflitto di route: `{id:int}` non combacia con "completed".
4. Aggiornare `api/README.md` con la riga della tabella.
5. `dotnet test api`.
6. Web non toccato (il contratto esistente non cambia; nessun uso richiesto dal thread, vedi D2).

## Fonti
- Richiesta del thread Teams.
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`.
- Nessun allegato in `/work/inputs` usato. Nessun `decisions.md` di altre feature presente.

## Rischio
Livello: giallo
- Costi: minimo, poche righe e due test.
- Complessità: bassa.
- Sicurezza: l'endpoint cancella dati senza autenticazione e CORS è aperto a ogni origine (`AllowAnyMethod`): qualsiasi pagina può svuotare i completati. Dati in memoria, progetto di prova, quindi accettabile.
- Accoppiamento: nessuna modifica al contratto esistente; il web non cambia.

## Domande aperte
- [ ] D1: Risposta attesa: 204 No Content, oppure 200 con il numero di todo eliminati? — fonti: richiesta (non specifica) vs convenzioni di `api/README.md`
- [ ] D2: Serve anche un pulsante «Elimina completati» nel web (`web/src/todos.js`)? — fonti: richiesta (solo API) vs CLAUDE.md (contratto riportato nel web)
