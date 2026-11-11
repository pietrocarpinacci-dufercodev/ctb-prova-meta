# T-1a23573b — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-11 | D1: data in formato ISO `AAAA-MM-GG` | bot (execute, `[approva comunque]`) | Domanda aperta; formato non ambiguo e ordinabile |
| 2026-11-11 | D2: riga `Ultimo aggiornamento: <data>` in fondo al README | bot (execute, `[approva comunque]`) | Domanda aperta; non altera il contenuto esistente |
| 2026-11-11 | D3: data del job di execute (`date +%F`), riga statica non rigenerata | bot (execute, `[approva comunque]`) | Domanda aperta; nessun automatismo richiesto |
| 2026-11-11 | Modifica fatta senza subagent `backend-dev` | bot | Solo documentazione, nessun codice |
| 2026-11-11 | D1 aggiornata: data e ora `AAAA-MM-GG HH:MM UTC` (ora del job di followup, fuso del container) | team (thread: «aggiungi anche l'ora») + bot | Richiesta nel thread; fuso esplicito per non essere ambigui |
