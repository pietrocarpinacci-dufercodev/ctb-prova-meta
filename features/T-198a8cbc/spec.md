# T-198a8cbc — Esportazione CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-198a8cbc (link non presente nel messaggio del job): "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati: `requisiti.pdf` ("la priorita ha i valori bassa, media e alta"), `dati.csv` (intestazione `titolo,priorita`, separatore virgola), `immagine-1.png` (immagine di pochi pixel, verde e uniforme, senza uno schema leggibile).

Il modello attuale `Todo(Id, Title, Done)` non ha la priorità: i criteri che la riguardano dipendono dalle domande aperte D1–D3 del piano.

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, separata da virgole come in `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare in memoria, in ordine di `Id` crescente, con il titolo e la priorità.
- WHILE non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- WHERE la priorità è presente, l'API SHALL scrivere solo uno dei valori `bassa`, `media`, `alta`.
- IF un titolo contiene virgola, doppi apici o a capo THEN l'API SHALL racchiuderlo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- IF un client chiama `POST /api/todos` con una priorità diversa da `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 (solo se D1 introduce la priorità nel modello).
- WHEN un client chiama `GET /api/todos/{id}` con un id numerico, l'API SHALL continuare a comportarsi come prima (la rotta `export` non deve interferire).
