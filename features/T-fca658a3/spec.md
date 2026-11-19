# T-fca658a3 — Export CSV dei todo (GET /api/todos/export)

## Richiesta
Dal thread Teams della feature T-fca658a3 (link non presente nel messaggio del job): "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati: `requisiti.pdf` ("la priorita ha i valori bassa, media e alta"), `dati.csv` (intestazione `titolo,priorita`, due righe di esempio), `immagine-1.png` (un riquadro verde uniforme di pochi pixel, senza uno schema leggibile).

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, come in `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni todo, ordinata per id, con il titolo e la priorità del todo.
- WHILE non ci sono todo, l'API SHALL restituire solo la riga di intestazione.
- WHEN si crea un todo con `POST /api/todos` senza priorità, l'API SHALL assegnare la priorità `media` (da confermare, D2).
- IF `POST /api/todos` riceve una priorità diversa da `bassa`, `media` o `alta` THEN l'API SHALL rispondere 400 con un ValidationProblem sul campo `priority`.
- IF un titolo contiene virgole, virgolette o a capo THEN l'API SHALL racchiudere il campo tra virgolette doppie e raddoppiare le virgolette interne (RFC 4180).
- WHEN un client chiama `GET /api/todos/{id}` con un id esistente, l'API SHALL continuare a rispondere come prima; `export` non SHALL essere interpretato come id.
