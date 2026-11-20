# T-7b336c71 — Esportazione CSV delle cose da fare

## Richiesta
Dal thread Teams della feature T-7b336c71: "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati (in `/work/inputs`):
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, righe `Comprare il latte,bassa` e `Preparare la demo,alta`.
- `immagine-1.png`: immagine 16×16 di colore verde uniforme, senza uno schema leggibile (vedi D1 in `plan.md`).

Oggi il modello `Todo` (`api/src/CtbProva.Api/Program.cs`) ha solo `Id`, `Title`, `Done`: la priorità non esiste e va aggiunta.

## Criteri di accettazione
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, identica a `dati.csv`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, nell'ordine di `GET /api/todos` (per id crescente), nella forma `<titolo>,<priorita>`.
- WHILE non ci sono cose da fare, l'API SHALL restituire la sola riga di intestazione.
- IF un titolo contiene virgola, doppio apice o a capo, THEN l'API SHALL racchiuderlo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- WHEN un client crea una cosa da fare con `POST /api/todos` e un campo `priority` con valore `bassa`, `media` o `alta`, l'API SHALL salvarla con quella priorità e restituirla nel JSON.
- WHEN un client crea una cosa da fare con `POST /api/todos` senza `priority`, l'API SHALL assegnare la priorità `media` (valore provvisorio, vedi D4).
- IF `priority` ha un valore diverso da `bassa`, `media`, `alta`, THEN l'API SHALL rispondere 400 con un errore di validazione sul campo `priority`.
- WHEN `/api/todos/export` viene chiamato, l'API SHALL non confonderlo con `/api/todos/{id}` (il vincolo `{id:int}` resta).
- WHERE l'endpoint di export è presente, `api/README.md` SHALL elencarlo nella tabella degli endpoint.
