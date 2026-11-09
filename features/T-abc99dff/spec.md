# T-abc99dff — Export CSV dei todo

## Richiesta
Endpoint `GET /api/todos/export` che esporti i todo in CSV, secondo i requisiti del PDF allegato (`requisiti.pdf`), il formato di `dati.csv` e lo schema dell'immagine allegata (non leggibile, vedi D3 in `plan.md`). Thread Teams: link non disponibile nel job.

## Criteri di accettazione
- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type` `text/csv`.
- WHEN l'export è richiesto, l'API SHALL emettere come prima riga l'intestazione `titolo,priorita`, con separatore virgola come in `dati.csv`.
- WHEN esistono dei todo, l'API SHALL emettere una riga per todo, in ordine di id.
- WHERE un titolo contiene virgola, virgolette o a-capo, l'API SHALL racchiuderlo tra virgolette raddoppiando le virgolette interne.
- WHILE non esiste alcun todo, l'API SHALL rispondere 200 con la sola intestazione.
- WHERE la priorità è presente, l'API SHALL esportarla con uno dei valori `bassa`, `media`, `alta` (subordinato a D1/D2).
- IF un client invia `GET /api/todos/{id}` con id numerico, THEN l'API SHALL continuare a comportarsi come prima, senza interferenze con `/api/todos/export`.
