# T-fd6fd233 — Piano

## Submodule coinvolti
- Nessuno: il documento sta nel meta-repo; nessuna modifica al codice di api e web.

## Passi
1. Scrivere `features/T-fd6fd233/test-plan.md` (max una pagina): obiettivo, ambito, livelli (unit web, integrazione API con `WebApplicationFactory`), comandi `dotnet test api` e `npm test --prefix web`.
2. Tabella dei casi coperti: API (health, crea+leggi+completa, titolo vuoto 400, id sconosciuto 404); web (`normalizeTitle`, `sortTodos`, `summary`, `addTodo` titolo vuoto e titolo ripulito).
3. Tabella dei casi mancanti da proporre: API `GET /api/todos` elenco, `/done` su id sconosciuto (404), titolo mancante/null; web errore HTTP in `addTodo` (`ok:false`), lista vuota in `summary`/`sortTodos`; coerenza contratto api↔web.
4. Segnalare cosa non è eseguibile nel container (es. prova manuale della pagina contro l'API su `localhost:5000`).
5. Non aggiungere test: l'implementazione dei casi mancanti sarebbe una feature separata.

## Fonti
- `CLAUDE.md` del meta-repo (comandi, contratto API)
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `web/tests/todos.test.js`
- `features/T-9dfe1d36/decisions.md` (nessuna contraddizione trovata)
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: minimi, un solo file di documentazione.
- Complessità: bassa.
- Sicurezza: nessun segreto né codice toccato.
- Accoppiamento: nessuno sui submodule.

## Domande aperte
- [ ] D1: "plan di test" è un documento (strategia + casi) o si vogliono anche i test mancanti implementati? Il piano assume solo il documento — fonti: richiesta del thread vs piano
- [ ] D2: Dove va il documento: `features/T-fd6fd233/test-plan.md` o un file stabile (es. `docs/test-plan.md` nel meta-repo)? — fonti: richiesta del thread vs struttura del meta-repo
