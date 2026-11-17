# T-2044f973 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-17 | Formato CSV preso da `dati.csv`: intestazione `titolo,priorita`, virgola come separatore, quoting RFC 4180 | bot (piano) | Unico formato concreto fornito; il quoting serve per titoli con virgole |
| 2026-11-17 | Endpoint minimal API in `Program.cs`, senza librerie CSV esterne | bot (piano) | Due colonne: bastano poche righe; nessuna dipendenza nuova |
| 2026-11-17 | Proposta (in attesa di D2–D5): aggiungere `Priority` al record `Todo`, valori `bassa`/`media`/`alta`, default `media` | bot (piano) | `Todo` oggi non ha la priorità che l'export deve scrivere |
