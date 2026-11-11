# T-a9462771 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-11 | Valori della priorità come stringhe italiane `bassa`/`media`/`alta` nel JSON, senza enum né convertitore (D3) | thread | Proposta del piano accettata; codice minimo, valori leggibili dal web così come arrivano |
| 2026-11-11 | Priorità di default `media`: usata dall'API se `priority` manca e preselezionata nel web (D1) | thread | Proposta del piano accettata |
| 2026-11-11 | Ordine dell'elenco invariato: API per id, web prima i da fare poi per id (D2) | thread | Proposta del piano accettata |
| 2026-11-11 | Modifica con `PUT /api/todos/{id}/priority` e corpo `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata |
| 2026-11-11 | Filtro `GET /api/todos?priority=<valore>` su un solo valore; valore non valido → 400 (D5) | thread | Proposta del piano accettata |
| 2026-11-11 | Il web mostra la priorità e la fa scegliere in creazione; niente filtro né modifica dal web (D6) | thread | Proposta del piano accettata |
| 2026-11-11 | Colori: bassa `#6b7383`, media `#b54708`, alta `#b42318`, sempre con il testo visibile (D7) | thread | Proposta del piano accettata |
| 2026-11-11 | Fuori ambito: persistenza, filtro su più valori, ordinamento per priorità | thread | Conseguenza di D2, D5, D6 |
