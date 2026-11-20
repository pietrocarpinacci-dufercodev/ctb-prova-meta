# T-198a8cbc — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-20 | CSV generato a mano con `StringBuilder` ed escape RFC 4180, senza librerie esterne | bot (piano) | Due colonne, nessuna dipendenza nuova da scaricare |
| 2026-11-20 | Intestazione e separatore presi da `dati.csv` (`titolo,priorita`, virgola) | bot (piano) | Formato indicato nel thread |
| 2026-11-20 | Ordine delle righe per `Id` crescente, come `TodoStore.All()` | bot (piano) | Coerenza con `GET /api/todos` |
