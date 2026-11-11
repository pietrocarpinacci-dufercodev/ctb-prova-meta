# T-4ab341d4 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-11 | La `priority` in creazione è opzionale (default da D1), il campo è additivo nel JSON | bot (piano) | Compatibilità con il web attuale e con client che non la inviano |
| 2026-11-11 | Valori non ammessi per `priority` (creazione, modifica, filtro) rispondono 400 | bot (piano) | Convenzione di validazione ai confini dell'API (`backend-dev.md`) |
| 2026-11-11 | Nel web la priorità è mostrata come testo oltre che a colori | bot (piano) | Accessibilità: il colore non deve essere l'unica indicazione |
