# T-9ee961f0 — Export CSV dei todo

## Richiesta
Il team chiede nel thread Teams un endpoint `GET /api/todos/export` che esporti i todo in CSV, secondo i requisiti del PDF allegato, il formato di `dati.csv` e lo schema dell'immagine allegata (illeggibile, vedi D2 nel piano). Il link al thread lo fornisce il bot.

## Criteri di accettazione
<!-- Provvisori: dipendono dalle risposte a D1–D5 in plan.md. -->
- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv`.
- WHEN l'export viene generato, l'API SHALL scrivere come prima riga l'intestazione `titolo,priorita` e poi una riga per ogni todo, ordinata per id.
- WHERE un todo ha la priorità, l'API SHALL esportarla con uno dei valori `bassa`, `media`, `alta`.
- IF un titolo contiene virgola, virgolette doppie o a capo THEN l'API SHALL racchiuderlo tra virgolette doppie, raddoppiando quelle interne.
- IF un titolo inizia con `=`, `+`, `-` o `@` THEN l'API SHALL neutralizzarlo nel CSV prefissandolo con `'`.
- WHILE non esistono todo, l'API SHALL restituire solo la riga di intestazione.
- WHEN `GET /api/todos/{id}` riceve un id numerico, l'API SHALL continuare a rispondere come prima, senza conflitti con `/api/todos/export`.
