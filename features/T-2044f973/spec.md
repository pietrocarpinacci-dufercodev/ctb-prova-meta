# T-2044f973 — Export CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-2044f973: "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati: `requisiti.pdf` ("la priorita ha i valori bassa, media e alta"), `dati.csv` (intestazione `titolo,priorita`, una riga per todo, separatore virgola), `immagine-1.png` (non leggibile: vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, identica a quella di `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, ordinata per `id` come `GET /api/todos`, con titolo e priorità separati da virgola.
- WHILE non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- IF un titolo contiene virgola, doppio apice o a capo, THEN l'API SHALL racchiuderlo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- IF un titolo inizia con `=`, `+`, `-` o `@`, THEN l'API SHALL anteporre un apice `'` per evitare la CSV injection nei fogli di calcolo (vedi D5).
- L'API SHALL scrivere nella colonna `priorita` solo uno dei valori `bassa`, `media`, `alta` (requisiti.pdf).
- WHEN una cosa da fare viene creata senza priorità, l'API SHALL assegnarle la priorità di default definita in D3.
- IF `POST /api/todos` riceve una priorità diversa da `bassa`, `media`, `alta`, THEN l'API SHALL rispondere 400 con un problema di validazione sul campo `priority`.
- La rotta `GET /api/todos/{id:int}` SHALL continuare a funzionare: `export` non è un intero e non va in conflitto.
