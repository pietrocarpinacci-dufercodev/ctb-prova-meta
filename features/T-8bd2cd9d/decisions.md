# T-8bd2cd9d — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-21 | Intestazione CSV `titolo,priorita` e valori `bassa`, `media`, `alta` presi dagli allegati `dati.csv` e `requisiti.pdf` | bot (piano) | Sono le uniche specifiche leggibili negli allegati |
| 2026-11-21 | Proposta: priorità come nuovo campo `priority` del modello, con validazione nel POST (da confermare, D2/D3) | bot (piano) | Il modello attuale non ha la priorità richiesta dal CSV |
| 2026-11-21 | Escaping dei campi secondo RFC 4180 | bot (piano) | I titoli sono testo libero e possono contenere virgole o doppi apici |
