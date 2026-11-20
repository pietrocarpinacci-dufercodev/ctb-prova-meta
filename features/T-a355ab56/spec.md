# T-a355ab56 — Export CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-a355ab56 (il link al thread non è disponibile nel job): "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati (in `/work/inputs`):
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, separatore virgola, righe di esempio `Comprare il latte,bassa` e `Preparare la demo,alta`.
- `immagine-1.png`: un quadrato verde senza testo né schema leggibile (vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, con la virgola come separatore, come in `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, nell'ordine di `GET /api/todos` (per id crescente).
- WHILE non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- L'API SHALL scrivere nella colonna `priorita` solo i valori `bassa`, `media` o `alta`.
- WHEN un client crea una cosa da fare con una priorità, l'API SHALL salvarla e restituirla nel JSON come campo `priority`.
- IF una cosa da fare viene creata senza priorità THEN l'API SHALL assegnarle il valore predefinito deciso in D3.
- IF una cosa da fare viene creata con una priorità diversa da `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 con un ValidationProblem sul campo `priority`.
- IF un titolo contiene virgola, doppi apici o a capo THEN l'API SHALL racchiudere il campo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- WHEN un client chiama `GET /api/todos/{id}` con un id intero, l'API SHALL continuare a rispondere come oggi (la rotta `export` non deve interferire).
