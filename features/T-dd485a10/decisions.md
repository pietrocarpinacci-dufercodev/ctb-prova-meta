# T-dd485a10 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-17 | `priority` come stringa validata contro un insieme fisso, non come enum C# | bot (piano) | Nessun converter JSON da configurare; i valori restano quelli del contratto |
| 2026-11-17 | Priorità opzionale in creazione, con default | bot (piano) | Il web attuale e i client esistenti continuano a funzionare con l'API nuova |
| 2026-11-17 | Nel web la priorità è testo colorato (`bassa` verde/grigio, `media` ambra, `alta` rosso), non solo colore | bot (piano), confermato nel thread | Accessibilità: il colore non deve essere l'unica indicazione |
| 2026-11-17 | Default `media`, anche preselezionato nel menu web (D1) | thread | Proposta del piano accettata |
| 2026-11-17 | Ordine dell'elenco invariato: API per id, web prima i non fatti poi per id (D2) | bot (piano), delega del thread | Il thread ha lasciato la scelta al bot: opzione più semplice |
| 2026-11-17 | Valori JSON `bassa`, `media`, `alta` minuscoli, confronto esatto; identificatori in inglese (D3) | thread | Proposta del piano accettata |
| 2026-11-17 | Endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata |
| 2026-11-17 | Filtro `GET /api/todos?priority=` su un solo valore, 400 se non ammesso (D5) | thread | Proposta del piano accettata |
| 2026-11-17 | Web: solo visualizzazione e scelta in creazione, niente modifica né filtro (D6) | thread | Proposta del piano accettata |
| 2026-11-17 | Fuori ambito: persistenza, ordinamento per priorità, filtro su più valori | bot (piano) | Non richiesti nel thread |
