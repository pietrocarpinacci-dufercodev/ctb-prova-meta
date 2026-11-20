# T-8060523e — Export CSV dei todo (GET /api/todos/export)

## Richiesta
Dal thread Teams della feature T-8060523e (link al thread non disponibile nel job): "Pianifica un endpoint GET /api/todos/export che esporti i todo in CSV seguendo i requisiti del PDF allegato, il formato del file dati.csv allegato e lo schema nell'immagine."

Allegati (in `/work/inputs`):
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, separatore virgola, due righe di esempio (`Comprare il latte,bassa`, `Preparare la demo,alta`).
- `immagine-1.png`: un quadrato verde uniforme di pochi pixel, senza schema leggibile (vedi D1 in `plan.md`).

Seconda richiesta nello stesso thread: "aggiungi al README una sezione "Glossario" con 40 voci tecniche del progetto", circa 400 caratteri per voce, con il testo completo riportato anche nel riepilogo del piano. Non è ancora chiaro se si aggiunge all'export o lo sostituisce (D9 in `plan.md`).

Stato attuale dell'API: il record `Todo(Id, Title, Done)` non ha la priorità e `POST /api/todos` accetta solo `title`.

## Criteri di accettazione
<!-- Bozza: i criteri marcati (D2)…(D6) dipendono dalle risposte alle domande aperte di plan.md. -->
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, come in `dati.csv` (D3).
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni todo, ordinate per id crescente, con il titolo e la priorità del todo.
- WHILE non ci sono todo, l'API SHALL restituire solo la riga di intestazione.
- WHEN un todo viene esportato, il campo priorita SHALL valere uno tra `bassa`, `media`, `alta` (requisiti.pdf).
- WHEN un client crea un todo con `POST /api/todos` indicando `priority`, l'API SHALL salvare quella priorità (D2).
- WHEN un client crea un todo con `POST /api/todos` senza `priority`, l'API SHALL assegnare la priorità `media` (D4).
- IF `POST /api/todos` riceve una priorità diversa da `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 con un ValidationProblem sul campo `priority` (D2).
- IF un titolo contiene virgola, doppi apici o a capo THEN l'API SHALL racchiudere il campo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180) (D5).
- WHEN un client chiama `GET /api/todos/export`, l'API SHALL inviare `Content-Disposition: attachment; filename="todos.csv"` (D5).
- WHEN un client chiama `GET /api/todos/{id}` con un id numerico, l'API SHALL continuare a comportarsi come oggi (la rotta `export` non deve interferire).
- WHERE è presente il glossario, il `README.md` del meta-repo SHALL contenere una sezione `## Glossario` con esattamente 40 voci, ciascuna con il termine in grassetto (D8, D11).
- WHERE è presente il glossario, ogni voce SHALL essere lunga circa 400 caratteri (tra 300 e 500) e descrivere un elemento che esiste nel codice o nei file del progetto al momento del commit (D10).
- IF una voce del glossario cita un endpoint, una funzione o un file THEN il nome SHALL coincidere con quello presente nel codice.
