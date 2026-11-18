# T-333ed6a2 — Esportazione CSV delle cose da fare

## Richiesta
Il team chiede di pianificare un endpoint `GET /api/todos/export` che esporti le cose da fare in CSV, seguendo i requisiti del PDF allegato (`requisiti.pdf`), il formato del file allegato (`dati.csv`) e lo schema dell'immagine allegata (`immagine-1.png`). Thread Teams della feature T-333ed6a2 (il link non è disponibile nel contesto del job).

Contenuto degli allegati:
- `requisiti.pdf`: "Requisiti: la priorita ha i valori bassa, media e alta."
- `dati.csv`: intestazione `titolo,priorita`, separatore virgola, fine riga LF, righe di esempio `Comprare il latte,bassa` e `Preparare la demo,alta`.
- `immagine-1.png`: un'immagine piccola e tutta verde, senza nessuno schema leggibile (vedi D1 in `plan.md`).

## Criteri di accettazione
<!-- Criteri provvisori: dipendono dalle risposte alle domande aperte D1–D6 di plan.md. -->
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL rispondere 200 con `Content-Type: text/csv; charset=utf-8`.
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL restituire come prima riga l'intestazione `titolo,priorita`, uguale a quella di `dati.csv`.
- WHEN il client chiama `GET /api/todos/export`, l'API SHALL restituire una riga per ogni cosa da fare, ordinate per `Id` crescente come `GET /api/todos`, con le righe che terminano in LF.
- WHILE non ci sono cose da fare, l'API SHALL restituire solo la riga di intestazione.
- WHERE una cosa da fare ha una priorità, l'API SHALL scrivere nella colonna `priorita` uno solo dei valori `bassa`, `media` o `alta`.
- IF un titolo contiene una virgola, un doppio apice o un a capo THEN l'API SHALL racchiudere il campo tra doppi apici e raddoppiare i doppi apici interni (RFC 4180).
- IF un titolo inizia con `=`, `+`, `-` o `@` THEN l'API SHALL neutralizzarlo come previsto dalla risposta a D6, per evitare l'iniezione di formule quando il file si apre in un foglio di calcolo.
- WHEN il client chiama `GET /api/todos/{id}` con un id numerico, l'API SHALL continuare a rispondere come prima: la rotta `export` non deve interferire con `{id:int}`.
