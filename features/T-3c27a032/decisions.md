# T-3c27a032 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-08 | Valori `priority` limitati a `bassa`, `media`, `alta` (minuscoli nel JSON, 400 per valori ignoti o `null`) | thread Teams | Dalla richiesta e dalla risposta a D5 |
| 2026-11-08 | Priorità di default `media` | thread Teams | Risposta a D1 |
| 2026-11-08 | Ordine dell'elenco invariato (non fatti prima, poi per id) in API e web | thread Teams | Risposta a D2 |
| 2026-11-08 | Endpoint di modifica `PUT /api/todos/{id}/priority` | thread Teams | Risposta a D3 |
| 2026-11-08 | Nel web solo visualizzazione e scelta alla creazione; filtro e modifica solo nell'API | thread Teams | Risposta a D4, aderente alla richiesta |
