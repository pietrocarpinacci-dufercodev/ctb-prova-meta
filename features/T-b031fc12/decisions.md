# T-b031fc12 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-12 | `priority` è una stringa nel JSON con i valori della richiesta (`bassa`, `media`, `alta`), senza enum né convertitori | bot (piano) | Valori indicati nel thread; codice minimo, coerente con i record esistenti |
| 2026-11-12 | Il filtro dell'elenco è un parametro di query opzionale `GET /api/todos?priority=<valore>` | bot (piano) | Non cambia l'endpoint esistente e resta compatibile con il web attuale |
| 2026-11-12 | Priorità non valida → 400, todo inesistente → 404 | bot (piano) | Convenzione di validazione ai confini in `.claude/agents/backend-dev.md` |
| 2026-11-12 | Nel web la priorità è mostrata come testo colorato, non solo colore | bot (piano) | Accessibilità |
| 2026-11-12 | Anche il lavoro sul web va a `backend-dev` | bot (piano) | `CLAUDE.md` del meta-repo; nessun agente Frontend (D4 di T-9dfe1d36) |
