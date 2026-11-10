# T-ae9fa8a0 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-10 | Priorità di default `media` (D1) | thread | Proposta del bot accettata |
| 2026-11-10 | Ordine: non fatti per priorità alta→bassa poi id, fatti in fondo; applicato nel web, API ordinata per id (D2) | thread | Proposta accettata; la scelta del lato (web) è interpretazione del bot: il minimo cambiamento, il web ordina già |
| 2026-11-10 | `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (D3) | thread | Proposta accettata |
| 2026-11-10 | Valori `bassa\|media\|alta`, filtro `?priority=`; input case-insensitive, output minuscolo (D4) | thread | Proposta accettata; case-insensitive è interpretazione del bot |
| 2026-11-10 | Web: solo menu in creazione e badge; niente modifica né filtro nel web (D5) | thread | Proposta accettata; segue la lettera della richiesta (interpretazione del bot) |
| 2026-11-10 | `priority` null in POST dà il default `media`; stringa vuota/spazi in POST, o mancante nel PUT, dà 400 | bot (execute) | Caso non coperto dalla spec: il default vale solo se il campo è assente |
| 2026-11-10 | Il web invia sempre `priority` (default `media` in `addTodo`); senza priorità valida il badge è vuoto e il todo va in fondo ai pari stato | bot (execute) | Tolleranza ai todo senza priorità prevista dalla spec |
