# T-b031fc12 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-12 | `priority` è una stringa nel JSON con i valori della richiesta (`bassa`, `media`, `alta`), senza enum né convertitori | bot (piano) | Valori indicati nel thread; codice minimo, coerente con i record esistenti |
| 2026-11-12 | Il filtro dell'elenco è un parametro di query opzionale `GET /api/todos?priority=<valore>` | bot (piano) | Non cambia l'endpoint esistente e resta compatibile con il web attuale |
| 2026-11-12 | Priorità non valida → 400, todo inesistente → 404 | bot (piano) | Convenzione di validazione ai confini in `.claude/agents/backend-dev.md` |
| 2026-11-12 | Nel web la priorità è mostrata come testo colorato, non solo colore | bot (piano) | Accessibilità |
| 2026-11-12 | Anche il lavoro sul web va a `backend-dev` | bot (piano) | `CLAUDE.md` del meta-repo; nessun agente Frontend (D4 di T-9dfe1d36) |
| 2026-11-12 | Priorità di default `media`, non obbligatoria in creazione; `media` preselezionata nel menu del web (D1) | thread | Proposta del piano accettata |
| 2026-11-12 | Ordine dell'elenco invariato: API per id, web prima i non fatti poi per id (D2) | thread | Proposta del piano accettata |
| 2026-11-12 | Endpoint dedicato `PUT /api/todos/{id}/priority` con corpo `{ "priority": "..." }` (D3) | thread | Proposta del piano accettata |
| 2026-11-12 | Nel web solo visualizzazione colorata e menu in creazione, niente filtro né modifica della priorità (D4) | thread | Proposta del piano accettata: solo quanto chiesto |
| 2026-11-12 | Colori: bassa grigio-blu, media ambra, alta rosso, con il testo sempre visibile (D5) | thread | Proposta del piano accettata |
| 2026-11-12 | Valori accettati solo esattamente `bassa`, `media`, `alta` in minuscolo; varianti → 400 (D6) | thread | Proposta del piano accettata |
| 2026-11-12 | `priority: ""` in creazione → 400; solo la priorità assente diventa `media` | bot (execute) | Un valore vuoto non è tra quelli ammessi (D6) |
| 2026-11-12 | La regola CSS dei todo fatti diventa `li.done span:not(.priority)`: il badge della priorità non viene barrato né ingrigito | bot (execute) | Leggibilità del badge colorato |
| 2026-11-12 | Colori: `#4a5d78` (bassa), `#8a5a00` (media), `#b42318` (alta), testo bianco | bot (execute) | D5 con contrasto stimato ≥ 4,5:1 |
| 2026-11-12 | Nessun helper per la classe CSS e `web/README.md` invariato | bot (execute) | Non servivano (passi 4 e 6 del piano) |
