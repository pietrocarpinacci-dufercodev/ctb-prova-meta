# T-9943caa7 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams di aggiungere una priorità ai todo: nell'API un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione e modificabile con un endpoint dedicato, con filtro dell'elenco per priorità; nel web la priorità accanto a ogni todo con colore diverso e un menu per sceglierla alla creazione. Priorità di default e ordine dell'elenco non sono ancora decisi (vedi domande aperte in `plan.md`).

## Criteri di accettazione
- WHEN un client crea un todo con `priority` valida (`bassa`, `media`, `alta`), l'API SHALL restituire 201 con il todo che contiene quel valore in `priority`.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità di default (da decidere, D1).
- IF `priority` in creazione non è uno dei tre valori ammessi THEN l'API SHALL rispondere 400 con errore di validazione su `priority` e non creare il todo.
- WHEN un client chiama l'endpoint dedicato di modifica priorità (forma da decidere, D3) con un valore valido su un todo esistente, l'API SHALL aggiornare `priority` e restituire il todo aggiornato.
- IF l'endpoint di modifica riceve un valore non ammesso THEN l'API SHALL rispondere 400 senza modificare il todo.
- IF l'endpoint di modifica riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con valore ammesso, l'API SHALL restituire solo i todo con quella priorità.
- IF `priority` nel filtro non è ammessa THEN l'API SHALL rispondere 400.
- WHEN `GET /api/todos` è chiamato senza filtro, l'API SHALL restituire tutti i todo nell'ordine deciso (D2).
- WHEN il web mostra un todo, il web SHALL mostrare la priorità accanto al titolo con un colore diverso per ciascuno dei tre valori.
- WHEN l'utente crea un todo dal web, il web SHALL inviare la priorità scelta nel menu, preselezionata sul default (D1).
- Il contratto aggiornato SHALL essere riportato in `api/README.md`, `web/src/todos.js` e nei test di entrambi i submodule.
