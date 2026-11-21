# T-34bae643 — Export CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-34bae643: «Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine.»

Allegati: `requisiti.pdf` («la priorita ha i valori bassa, media e alta»), `dati.csv` (intestazione `titolo,priorita`, due righe di esempio), `immagine-1.png` (nessuno schema leggibile, vedi D1 nel piano).

## Criteri di accettazione
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, come in `dati.csv`.
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, ordinate per id crescente come `GET /api/todos`, con il titolo e la priorità.
- WHILE l'elenco è vuoto, l'API SHALL restituire solo la riga di intestazione.
- L'API SHALL scrivere la priorità solo con uno dei valori `bassa`, `media`, `alta` (requisiti.pdf).
- IF un titolo contiene virgola, doppio apice o a capo THEN l'API SHALL racchiuderlo tra doppi apici raddoppiando i doppi apici interni (RFC 4180).
- WHEN il client chiama `GET /api/todos/export`, `GET /api/todos/{id}` SHALL continuare a funzionare come prima (la rotta `export` non deve essere interpretata come id).
