# T-7615889e — Esportazione CSV dei todo

## Richiesta
Il team chiede un endpoint `GET /api/todos/export` che esporti i todo in CSV, seguendo i requisiti del PDF allegato (`requisiti.pdf`), il formato di `dati.csv` e lo schema dell'immagine allegata (`immagine-1.png`, non leggibile: vedi D3 in `plan.md`). Link al thread Teams: non fornito nel job.

## Criteri di accettazione
<!-- Bozza: dipende dalle risposte a D1–D5 in plan.md. -->
- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv` e un CSV con intestazione `titolo,priorita` e una riga per ogni todo, ordinata per id.
- WHEN non esistono todo, l'API SHALL rispondere 200 con il solo CSV di intestazione.
- WHERE un titolo contiene virgola, virgolette doppie o a capo, l'API SHALL racchiuderlo tra virgolette raddoppiando quelle interne (RFC 4180).
- WHERE la priorità è presente (D1), l'API SHALL esportarla con uno dei valori `bassa`, `media`, `alta`.
- IF viene richiesto un id non numerico diverso da `export`, THEN le rotte esistenti SHALL continuare a rispondere come prima (nessuna regressione su `/api/todos/{id}`).
