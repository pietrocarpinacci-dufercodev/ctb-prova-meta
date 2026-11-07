# T-a38929d2 — Cancellazione dei todo

## Richiesta
Nel thread: «pianifica la cancellazione dei todo seguendo i requisiti nel PDF allegato e il mockup incollato; usa il CSV come dati di esempio per i test. Nel piano cita i file usati nelle Fonti.»

**Stato: bozza non approvabile.** Il PDF, il mockup e il CSV non sono arrivati al job: `/work/inputs` è vuoto e nel repo non ce n'è traccia. I criteri sotto vengono solo dalla richiesta e dal codice esistente. Vanno confermati o sostituiti quando gli allegati saranno disponibili (vedi D1–D3 in `plan.md`).

## Criteri di accettazione
Provvisori, da riscrivere sul PDF.

- WHEN arriva `DELETE /api/todos/{id}` con un id esistente, l'API SHALL rimuovere il todo e rispondere con uno status di successo (204 oppure 200: vedi D4).
- IF l'id non esiste THEN l'API SHALL rispondere 404.
- WHEN un todo è stato cancellato, `GET /api/todos` e `GET /api/todos/{id}` SHALL non restituirlo più (quest'ultimo con 404).
- WHEN l'utente chiede di cancellare un todo dalla pagina, il web SHALL chiamare `DELETE /api/todos/{id}` e ricaricare l'elenco.
- IF la cancellazione fallisce THEN il web SHALL mostrare il messaggio di errore nell'elemento `#error`.
- Il contratto dell'API SHALL restare uguale in `api/README.md`, `web/src/todos.js` e nei test di entrambi i submodule.

## Fonti
- Richiesta del thread (testo).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`: stato attuale del contratto.
- PDF dei requisiti, mockup, CSV: **non ricevuti**, non usati.
