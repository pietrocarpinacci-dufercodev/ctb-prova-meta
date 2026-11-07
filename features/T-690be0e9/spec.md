# T-690be0e9 — Cancellazione dei todo

## Richiesta
Pianificare la cancellazione dei todo seguendo i requisiti del PDF allegato e il mockup incollato; usare il CSV come dati di esempio per i test (richiesta nel thread Teams).

**Stato: bozza.** Gli allegati (PDF, CSV, mockup) non sono arrivati al job: `/work/inputs` è vuota e non c'è nulla di riferibile al feature id nel container. I criteri sotto sono un minimo ricavato dal solo codice esistente; i requisiti reali vanno confermati (vedi `plan.md`, Domande aperte).

## Fonti
- Richiesta nel thread (testo sopra).
- PDF dei requisiti: NON ricevuto.
- Mockup incollato: NON ricevuto.
- CSV dei dati di esempio: NON ricevuto.
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` (contratto attuale, nessun DELETE).
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`.
- `CLAUDE.md` del meta-repo (un cambio di contratto va riportato in `web/src/todos.js` e nei test di entrambi).

## Criteri di accettazione (provvisori)
- WHEN un client invia `DELETE /api/todos/{id}` per un todo esistente, l'API SHALL rimuoverlo e rispondere 204 (da confermare: D2).
- IF l'id non esiste THEN l'API SHALL rispondere 404.
- WHEN l'utente conferma la cancellazione di un todo nella pagina, il web SHALL chiamare l'API e ridisegnare l'elenco senza quel todo (forma esatta dal mockup: D1, D3).
- IF l'API risponde con errore THEN il web SHALL mostrare il messaggio nell'area errori esistente e lasciare l'elenco invariato.
