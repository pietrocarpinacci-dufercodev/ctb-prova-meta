# T-74e91fe0 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-09 | Priorità di default `media` | team (thread Teams) | proposta del piano accettata (D1) |
| 2026-11-09 | Web: non fatti prima, poi priorità alta→bassa, poi id; API ordinata per id | team (thread Teams) | proposta accettata (D2); API invariata per minimo cambiamento |
| 2026-11-09 | `PUT /api/todos/{id}/priority` con `{ "priority": "..." }`; nel web la priorità si sceglie solo alla creazione | team (thread Teams) | proposta accettata (D3) |
| 2026-11-09 | Filtro `?priority=` solo in API; valore sconosciuto → 400 | team (thread Teams) | proposta accettata (D4) |
| 2026-11-09 | Valori `bassa`/`media`/`alta`, input case-insensitive, output minuscolo | team (thread Teams) | proposta accettata (D5) |
