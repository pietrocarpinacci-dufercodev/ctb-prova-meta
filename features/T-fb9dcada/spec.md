# T-fb9dcada — Export CSV dei todo (GET /api/todos/export)

## Richiesta
Dal thread Teams della feature T-fb9dcada (link al thread non disponibile nel job): "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati: `requisiti.pdf` ("la priorita ha i valori bassa, media e alta"), `dati.csv` (intestazione `titolo,priorita`, due righe di esempio), `immagine-1.png` (un quadrato verde di pochi pixel: non contiene uno schema leggibile, vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, nell'ordine di `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni todo, ordinate per id come `GET /api/todos`.
- WHILE non esistono todo, l'API SHALL restituire solo la riga di intestazione.
- WHEN un todo viene esportato, l'API SHALL scrivere nella colonna `priorita` solo uno dei valori `bassa`, `media`, `alta`.
- IF un titolo contiene virgola, doppio apice o a capo THEN l'API SHALL racchiuderlo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL continuare a servire `GET /api/todos/{id}` per gli id numerici senza conflitti di route.
