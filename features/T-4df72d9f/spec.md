# T-4df72d9f — Export CSV dei todo

## Richiesta
Il team ha chiesto nel thread Teams un endpoint `GET /api/todos/export` che esporti i todo in CSV, seguendo i requisiti del PDF (`requisiti.pdf`), il formato di `dati.csv` e lo schema dell'immagine (`immagine-1.png`). Link al thread: non fornito al runner.

## Criteri di accettazione
Soggetti a risposta alle domande aperte di `plan.md` (colonne, campo priorità, schema immagine).
- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv` e un CSV con riga di intestazione seguita da una riga per todo, ordinate per Id.
- WHEN non esistono todo, l'API SHALL rispondere 200 con la sola riga di intestazione.
- WHERE un titolo contiene virgole, virgolette o a capo, l'API SHALL quotarlo secondo RFC 4180 (virgolette raddoppiate).
- WHERE un titolo inizia con `=`, `+`, `-` o `@`, l'API SHALL neutralizzarlo contro la formula injection (prefisso `'`).
- WHERE la priorità è esportata, l'API SHALL usare solo i valori `bassa`, `media`, `alta` (da `requisiti.pdf`).
- IF `GET /api/todos/{id}` riceve `export`, THEN l'API SHALL continuare a servire l'endpoint di export (il vincolo `{id:int}` evita il conflitto di route).
