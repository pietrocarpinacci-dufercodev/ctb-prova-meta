# T-fca658a3 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-19 | Le colonne e l'intestazione del CSV sono quelle di `dati.csv` (`titolo,priorita`), in attesa di D1 e D4 | bot (piano) | L'immagine allegata non contiene uno schema leggibile |
| 2026-11-19 | Il CSV si genera a mano con `StringBuilder` ed escape RFC 4180, senza librerie nuove | bot (piano) | Due colonne non giustificano una dipendenza |
| 2026-11-19 | La route `/api/todos/export` convive con `/api/todos/{id:int}` grazie al vincolo `int` | bot (piano) | Verificato su `Program.cs` |
