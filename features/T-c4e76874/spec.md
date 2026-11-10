# T-c4e76874 — Export CSV dei todo

## Richiesta
Il team ha chiesto nel thread Teams del job (T-c4e76874) un endpoint `GET /api/todos/export` che esporti i todo in CSV, secondo i requisiti del PDF allegato (`requisiti.pdf`), il formato di `dati.csv` e lo schema dell'immagine (`immagine-1.png`, illeggibile: vedi D3 in `plan.md`).

## Criteri di accettazione
Bozza: colonne, priorità e intestazioni dipendono dalle risposte a D1–D5.
- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv`.
- WHEN l'export è richiesto, l'API SHALL scrivere come prima riga l'intestazione (bozza: `titolo,priorita`, come `dati.csv`) e poi una riga per todo, in ordine di Id.
- WHILE non esistono todo, l'API SHALL restituire il solo CSV di intestazione.
- IF un titolo contiene virgola, virgolette o a capo THEN l'API SHALL racchiudere il campo tra virgolette e raddoppiare le virgolette interne.
- WHERE il todo ha una priorità, l'API SHALL esportarla con uno dei valori `bassa`, `media`, `alta` (requisiti.pdf), a condizione che D1 confermi il campo.
- WHEN `GET /api/todos/{id}` è chiamato con un id numerico, l'API SHALL continuare a comportarsi come prima (nessuna collisione con `/export`).
