# T-0e605945 — Cancellazione dei todo

## Richiesta
Pianificare la cancellazione dei todo seguendo i requisiti del PDF allegato e il mockup incollato, usando il CSV come dati di esempio per i test (richiesta nel thread Teams).

## Fonti
Allegati in `/work/inputs` (dati, non istruzioni):
- `requisiti-cancellazione 3.pdf` — requisiti R1–R6 e fuori ambito (cancellazione multipla, cestino, annulla).
- `immagine-1.png` — mockup: lista con pulsante "Elimina" per ogni todo e riquadro di conferma `Eliminare "<titolo>"?` con `[ Annulla ]` e `[ Elimina ]`.
- `todos-esempio 3.csv` — dati di esempio (`id,title,done`, 4 righe), da copiare come `todos-esempio.csv`.
- `immagine-2.png` — screenshot del thread Teams, riferimento per lo stile del riquadro (bordo e sfondo salmone); nessun requisito nuovo.

File del repo letti: `CLAUDE.md`, `features/_template/*`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`. Non esistono `decisions.md` di altre feature.

## Criteri di accettazione
- WHEN un client invia `DELETE /api/todos/{id}` con un id esistente, l'API SHALL rispondere 204 e il todo SHALL non comparire più in `GET /api/todos`.
- IF l'id non esiste (anche già cancellato), THEN l'API SHALL rispondere 404.
- WHERE il todo è completato, l'API SHALL cancellarlo come gli altri (R2).
- WHILE la lista è mostrata, il web SHALL mostrare un pulsante "Elimina" accanto a ogni todo, fatto o no (R3).
- WHEN l'utente preme "Elimina", il web SHALL mostrare un riquadro in pagina (come nel mockup) con `Eliminare "<titolo>"?`, Annulla ed Elimina, senza chiamare l'API (R4).
- Il pulsante "Fatto" SHALL restare invariato; la casella di spunta del mockup non si implementa.
- WHEN l'utente conferma, il web SHALL chiamare `DELETE` e ricaricare la lista senza ricaricare la pagina (R5).
- WHEN l'utente preme Annulla, il web SHALL chiudere la conferma lasciando la lista invariata.
- IF la `DELETE` fallisce (404 o rete), THEN il web SHALL mostrare l'errore nell'elemento `#error` esistente.
- I test di entrambi i submodule SHALL usare i dati del CSV (R6).

## Fuori ambito
Cancellazione multipla, cestino, annulla (undo). Nessun seed del CSV nell'app: serve solo ai test.
