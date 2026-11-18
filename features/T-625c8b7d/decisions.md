# T-625c8b7d — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-18 | Priorità di default `media`, campo facoltativo in creazione (D1) | thread | Proposta del piano accettata |
| 2026-11-18 | Ordine dell'elenco invariato: API per id, web prima i non fatti poi per id (D2) | bot (piano), su delega del thread | Scelta più semplice: nessun cambio di ordinamento |
| 2026-11-18 | Valori nel JSON in italiano `bassa`/`media`/`alta`, mostrati così nel web senza mappatura; identificatori del codice in inglese (D3) | bot (piano), su delega del thread | Scelta più semplice: i valori sono quelli del thread e non serve una traduzione tra api e web |
| 2026-11-18 | Endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata; PUT perché imposta un valore, a differenza di `/done` che è un'azione |
| 2026-11-18 | Filtro `GET /api/todos?priority=<valore>` su un solo valore, 400 se non valido (D5) | thread | Proposta del piano accettata |
| 2026-11-18 | Nel web nessun filtro né modifica della priorità dopo la creazione (D6) | thread | Proposta del piano accettata |
| 2026-11-18 | Colori nel web: bassa verde, media ambra, alta rosso (D7) | thread | Proposta del piano accettata |
| 2026-11-18 | La priorità nel web è mostrata anche come testo, non solo col colore | bot (piano) | Accessibilità: il colore da solo non basta |
| 2026-11-18 | In `PUT /api/todos/{id}/priority` la validazione viene prima della ricerca: id inesistente con priorità non valida risponde 400, non 404 | bot (execute) | Stesso ordine di `POST /api/todos` (prima la validazione del body); la spec non definisce il caso combinato |
| 2026-11-18 | Il messaggio d'errore nuovo del web (`Invalid priority.`) e l'`aria-label` del menu (`Priority`) sono in inglese, anche se i testi esistenti del web sono in italiano | bot (execute) | Regola di lingua del progetto: testi e messaggi nel codice in inglese |
| 2026-11-18 | Il lavoro sul codice è stato delegato al subagent `backend-dev`, anche per il web | bot (execute) | Regola del `CLAUDE.md` del meta-repo; non esiste un agente Frontend (T-9dfe1d36, D4) |
