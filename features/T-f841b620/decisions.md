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
| 2026-11-18 | `priority` accettata senza distinzione maiuscole/minuscole, solo come uno dei tre nomi: numeri ("1") e liste ("low,high") danno 400 | bot (execute) | `Enum.TryParse` da solo accetterebbe anche numeri e liste |
| 2026-11-18 | Messaggio di errore della priorità in inglese; quello esistente del titolo resta in italiano | bot (execute) | Regola delle lingue: testo nuovo nel codice in inglese, testo esistente invariato |
| 2026-11-18 | `MarkDone` e `SetPriority` condividono il metodo privato `TodoStore.Update` | bot (execute) | Evita di duplicare la stessa logica |
| 2026-11-18 | Nei todo fatti l'etichetta della priorità non è barrata né ingrigita (`li.done span:not(.priority)`) | bot (execute) | Mantiene il contrasto dell'etichetta |
| 2026-11-18 | Nel web una priorità assente o sconosciuta si ordina come `medium` e si mostra col valore grezzo | bot (execute) | Robustezza verso dati inattesi senza codice in più |
| 2026-11-18 | Implementazione delegata al subagent `backend-dev` anche per il web | bot (execute) | `CLAUDE.md` e D7 |
