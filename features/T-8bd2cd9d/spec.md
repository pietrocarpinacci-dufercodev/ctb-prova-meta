# T-8bd2cd9d — Esportazione CSV delle cose da fare

## Richiesta
Nel thread Teams della feature T-8bd2cd9d è stato chiesto: "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati (in `/work/inputs`):
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, righe di esempio `Comprare il latte,bassa` e `Preparare la demo,alta`.
- `immagine-1.png`: un'immagine di pochi pixel a tinta unita verde, senza uno schema leggibile (vedi D1 nel piano).

Il modello attuale `Todo(Id, Title, Done)` non ha la priorità: l'esportazione richiede di introdurla (vedi D2 nel piano).

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, come in `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, nell'ordine di `id` crescente, con il titolo e la priorità.
- WHEN non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL includere l'intestazione `Content-Disposition: attachment; filename="todos.csv"`.
- IF un titolo contiene virgola, doppio apice o a capo, THEN l'API SHALL racchiudere il campo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- WHEN un client esporta il CSV, ogni valore della colonna `priorita` SHALL essere uno tra `bassa`, `media` e `alta`, come da `requisiti.pdf`.
- WHEN un client crea una cosa da fare con `POST /api/todos` senza priorità, l'API SHALL assegnare la priorità `media` (proposta, vedi D3).
- IF un client crea una cosa da fare con una priorità diversa da `bassa`, `media` o `alta`, THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority`.
- WHEN un client chiama `GET /api/todos/{id}` con un id numerico, l'API SHALL continuare a comportarsi come prima: la rotta `export` non deve intercettarla né esserne intercettata.
