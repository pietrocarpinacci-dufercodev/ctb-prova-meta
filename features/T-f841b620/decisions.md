# T-f841b620 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-18 | La priorità nel web è mostrata come testo colorato (Bassa/Media/Alta), non solo come colore | bot (piano) | Accessibilità: l'informazione non deve dipendere solo dal colore |
| 2026-11-18 | Nessuna dipendenza nuova: enum serializzato con `JsonStringEnumConverter` di `System.Text.Json` | bot (piano) | Già disponibile nel framework |
| 2026-11-18 | Priorità di default `medium`, preselezionata anche nel menu del web (D1) | thread | Proposta del piano accettata |
| 2026-11-18 | Ordine: nel web non fatti prima, poi priorità alta→bassa, poi id; l'API resta per id (D2) | thread | Proposta del piano accettata |
| 2026-11-18 | Valori JSON in inglese `low`, `medium`, `high`, etichette italiane solo nel web (D3) | thread | Regola delle lingue: codice in inglese |
| 2026-11-18 | Endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata |
| 2026-11-18 | Valore di priorità non ammesso → 400 in creazione, endpoint dedicato e filtro (D5) | thread | Coerente con la validazione del titolo |
| 2026-11-18 | Nel web solo visualizzazione e scelta in creazione, niente filtro né modifica (D6) | thread | Fuori dalla richiesta |
| 2026-11-18 | Anche la parte web è delegata a `backend-dev` (D7) | thread | Nessun agente Frontend (T-9dfe1d36 D4) |
