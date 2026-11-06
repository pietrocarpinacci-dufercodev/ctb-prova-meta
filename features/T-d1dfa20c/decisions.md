# T-d1dfa20c — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-06 | Valori della priorità in inglese: `high`, `medium`, `low` (D1) | thread | Risposta nel thread; coerente con i campi in inglese dell'API |
| 2026-11-06 | La priorità si imposta alla creazione con `POST /api/todos` e si modifica con `PATCH /api/todos/{id}`; nessun endpoint `/priority` dedicato (D2) | thread | Risposta nel thread |
| 2026-11-06 | In `POST /api/todos` la priorità è obbligatoria, nessun valore predefinito (D3) | thread | Risposta nel thread |
| 2026-11-06 | `PATCH` accetta gli stessi campi del POST (`title`, `priority`): campo assente = invariato, presente = aggiornato, `null` = messo a null se il campo lo prevede (D4) | thread | Risposta nel thread |
| 2026-11-06 | L'elenco si ordina anche per priorità (D5) | thread | Risposta nel thread |
| 2026-11-06 | La priorità di una voce già fatta si può cambiare con il PATCH (D6) | thread | Risposta nel thread |
