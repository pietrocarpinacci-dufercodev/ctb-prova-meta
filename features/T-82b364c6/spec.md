# T-82b364c6 — Export CSV dei todo

## Richiesta
Il team chiede un endpoint `GET /api/todos/export` che esporti i todo in CSV, seguendo i requisiti del PDF allegato, il formato di `dati.csv` e lo schema dell'immagine allegata (thread Teams della feature T-82b364c6).

## Criteri di accettazione
Provvisori: dipendono dalle risposte alle domande aperte di `plan.md` (colonne, priorità, immagine).

- WHEN un client invia `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv` e un corpo CSV.
- Il CSV SHALL avere una riga di intestazione `titolo,priorita` (come `dati.csv`) seguita da una riga per ogni todo, in ordine di `Id`.
- WHEN non esistono todo, l'API SHALL restituire solo la riga di intestazione.
- IF un titolo contiene virgole, virgolette o a capo THEN l'API SHALL quotare il campo secondo RFC 4180.
- IF un titolo inizia con `=`, `+`, `-` o `@` THEN l'API SHALL neutralizzarlo contro la formula injection nei fogli di calcolo (da confermare in D5).
- La priorità SHALL assumere solo i valori `bassa`, `media`, `alta` (PDF).
- Il percorso `/api/todos/export` SHALL non entrare in conflitto con `/api/todos/{id:int}`.
