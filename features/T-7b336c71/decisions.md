# T-7b336c71 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-20 | Intestazione CSV e valori di priorità presi alla lettera da `dati.csv` e `requisiti.pdf` (`titolo,priorita`; `bassa`, `media`, `alta`) | bot (piano) | Unici formati leggibili negli allegati |
| 2026-11-20 | CSV generato con `StringBuilder` ed escape RFC 4180, senza librerie nuove | bot (piano) | Due colonne non giustificano una dipendenza |
| 2026-11-20 | La rotta `/api/todos/export` convive con `/api/todos/{id:int}` grazie al vincolo `int` già presente | bot (piano) | Nessuna modifica alle rotte esistenti |
