# T-d1dfa20c — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-06 | Valori della priorità in inglese: `high`, `medium`, `low` (D1) | thread | Risposta nel thread; coerente con i campi in inglese dell'API |
| 2026-11-06 | La priorità si imposta alla creazione con `POST /api/todos` e si modifica con `PATCH /api/todos/{id}`; nessun endpoint `/priority` dedicato (D2) | thread | Risposta nel thread |
| 2026-11-06 | In `POST /api/todos` la priorità è obbligatoria, nessun valore predefinito (D3) | thread | Risposta nel thread |
| 2026-11-06 | `PATCH` accetta gli stessi campi del POST (`title`, `priority`): campo assente = invariato, presente = aggiornato, `null` = messo a null se il campo lo prevede (D4) | thread | Risposta nel thread |
| 2026-11-06 | L'elenco si ordina anche per priorità (D5) | thread | Risposta nel thread |
| 2026-11-06 | La priorità di una voce già fatta si può cambiare con il PATCH (D6) | thread | Risposta nel thread |
| 2026-11-06 | La priorità si può rimuovere: `priority` ammette `null`; `PATCH` con `"priority": null` la azzera. `title` resta non nullable (D7) | thread | Risposta nel thread |
| 2026-11-06 | In `POST /api/todos` `priority` può essere `null` esplicito: si crea una cosa da fare senza priorità (D9) | thread | Risposta nel thread |
| 2026-11-06 | In POST tutti i campi devono essere sempre presenti, anche `null` dove ammesso; l'assenza di un campo è prevista solo nel PATCH (campo non aggiornato). POST con `priority` assente = 400 (D10) | thread | Risposta nel thread |
| 2026-11-06 | Ordinamento: non fatte prima, poi `high`→`medium`→`low`→senza priorità (null) in fondo, poi id. "low → low" nella risposta è letto come `high → medium → low` (D8) | thread | Risposta nel thread |
