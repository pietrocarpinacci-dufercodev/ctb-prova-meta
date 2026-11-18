# T-625c8b7d — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-18 | Proposta: endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }`, in attesa di conferma (D4) | bot (piano) | Il thread chiede un endpoint dedicato senza indicarne la forma; PUT perché imposta un valore, a differenza di `/done` che è un'azione |
| 2026-11-18 | Proposta: filtro con query string `GET /api/todos?priority=<valore>` su un solo valore, 400 se non valido, in attesa di conferma (D5) | bot (piano) | Forma minima che soddisfa la richiesta senza nuovi endpoint |
| 2026-11-18 | Nel web nessun filtro né modifica della priorità dopo la creazione, salvo risposta diversa a D6 | bot (piano) | Il thread chiede solo visualizzazione e scelta in creazione |
| 2026-11-18 | La priorità nel web è mostrata anche come testo, non solo col colore | bot (piano) | Accessibilità: il colore da solo non basta |
