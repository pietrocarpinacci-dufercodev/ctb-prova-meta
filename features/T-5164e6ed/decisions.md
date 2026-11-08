# T-5164e6ed — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-07 | D1: i todo completati non sono scaduti e non vanno evidenziati | team (thread Teams) | risposta alla domanda aperta sul comportamento dei completati |
| 2026-11-07 | D2: "oggi" è la data UTC del server per l'API e la data locale del browser per il web; un todo che scade oggi non è scaduto | team (thread Teams) | `dueDate` è solo una data, senza fuso: serve fissare il riferimento |
| 2026-11-07 | D3: l'endpoint dei scaduti è `GET /api/todos/overdue` | team (thread Teams) | nome scelto tra le due alternative proposte |
| 2026-11-07 | D4: il form del web ha un campo data facoltativo | team (thread Teams) | permettere di impostare la scadenza dal web |
| 2026-11-08 | `GET /api/todos/overdue` ordina per `dueDate` crescente (il più scaduto per primo); a parità di data per id | team (thread Teams) | richiesta di followup; sostituisce l'ordinamento per id previsto in origine. Il pari-data per id è una scelta mia, non detta nel thread |
