# T-144b7b69 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-21 | Nell'API la priorità resta una stringa del JSON validata su un elenco chiuso, senza enum né converter | bot (piano) | Codice minimo; il valore resta nella forma del contratto |
| 2026-11-21 | Nel web la priorità si mostra con un'etichetta di testo oltre al colore | bot (piano) | Accessibilità: il colore da solo non basta |
| 2026-11-21 | `priority` è facoltativo in `POST /api/todos`: il contratto resta compatibile con i client esistenti | bot (piano) | Nessuna rottura per chi crea todo senza priorità |
