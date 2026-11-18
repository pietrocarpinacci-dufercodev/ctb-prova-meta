# T-5e3a8542 — Esportazione CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-5e3a8542: "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati (in `/work/inputs`):
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, righe di esempio `Comprare il latte,bassa` e `Preparare la demo,alta`.
- `immagine-1.png`: quadrato verde di pochi pixel, nessuno schema leggibile (vedi D1 in `plan.md`).

Il modello attuale `Todo(Id, Title, Done)` non ha la priorità: l'export richiede di aggiungerla (proposta da confermare, D2–D4 in `plan.md`).

## Criteri di accettazione
<!-- I criteri marcati (proposta) dipendono dalle domande aperte di plan.md e vanno confermati. -->
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, identica a `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare in memoria, ordinate per id crescente come `GET /api/todos`, con il titolo e la priorità.
- WHILE non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- IF un titolo contiene virgola, doppi apici o a capo THEN l'API SHALL racchiuderlo tra doppi apici raddoppiando quelli interni (RFC 4180).
- IF un titolo inizia con `=`, `+`, `-` o `@` THEN l'API SHALL neutralizzarlo contro la CSV injection (proposta, D6).
- WHEN un client crea una cosa da fare con `POST /api/todos`, l'API SHALL accettare un campo opzionale `priority` con valori `bassa`, `media` o `alta` (proposta, D2–D3).
- IF `priority` manca THEN l'API SHALL assegnare `media` (proposta, D3).
- IF `priority` ha un valore diverso da `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 con un ValidationProblem sul campo `priority`.
- WHEN un client legge le cose da fare (`GET /api/todos`, `GET /api/todos/{id}`), l'API SHALL includere il campo `priority` nel JSON.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL impostare `Content-Disposition: attachment; filename="todos.csv"` (proposta, D5).
