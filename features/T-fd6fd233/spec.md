# T-fd6fd233 — Breve plan di test

## Richiesta
Il team ha chiesto nel thread Teams: "un breve plan di test". Interpretazione provvisoria (vedi D1 in plan.md): un documento breve che descrive la strategia e i casi di test di api e web, senza nuovo codice di test.

## Criteri di accettazione
- WHEN il piano è eseguito, il meta-repo SHALL contenere un documento di test plan di al massimo una pagina.
- Il documento SHALL elencare, per api e web, i casi già coperti dai test esistenti (`api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `web/tests/todos.test.js`) e i casi mancanti.
- Il documento SHALL indicare i comandi di esecuzione `dotnet test api` e `npm test --prefix web`.
- WHERE il contratto API cambia, il documento SHALL richiedere test coerenti in api e web.
- IF un caso non è eseguibile nel container THEN il documento SHALL indicarlo come non eseguito.
