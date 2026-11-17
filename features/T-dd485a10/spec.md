# T-dd485a10 — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-dd485a10: aggiungere una priorità ai todo.
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità.
- Web: priorità mostrata accanto a ogni todo con un colore diverso per valore e un menu per sceglierla quando si crea un todo.
- Non ancora decisi: la priorità di default e l'ordine dell'elenco (vedi D1 e D2 in `plan.md`).

## Criteri di accettazione
Nota: i valori della priorità nel JSON dipendono da D3 e il percorso dell'endpoint dedicato da D4.

- Il sistema (API) SHALL restituire in ogni todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done`) il campo `priority` con uno dei tre valori ammessi.
- WHEN un client crea un todo con `POST /api/todos` e un `priority` valido, l'API SHALL salvare e restituire quella priorità con risposta 201.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità di default decisa in D1.
- IF `POST /api/todos` riceve un `priority` non ammesso THEN l'API SHALL rispondere 400 con un ValidationProblem sulla chiave `priority`, senza creare il todo.
- WHEN un client invia una priorità valida all'endpoint dedicato (D4) per un todo esistente, l'API SHALL aggiornare la priorità e restituire il todo aggiornato con risposta 200, senza cambiare titolo e stato `done`.
- IF l'endpoint dedicato riceve un id inesistente THEN l'API SHALL rispondere 404.
- IF l'endpoint dedicato riceve una priorità non ammessa o assente THEN l'API SHALL rispondere 400 con un ValidationProblem sulla chiave `priority`.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con un valore ammesso, l'API SHALL restituire solo i todo con quella priorità.
- WHEN un client chiama `GET /api/todos` senza `priority`, l'API SHALL restituire tutti i todo come oggi.
- IF `GET /api/todos` riceve un `priority` non ammesso THEN l'API SHALL rispondere 400 (salvo diversa risposta a D5).
- Il sistema (web) SHALL mostrare la priorità accanto a ogni todo come testo, con un colore diverso per ciascuno dei tre valori (il colore non è l'unica indicazione).
- WHERE il form di creazione è presente, il web SHALL offrire un menu `<select>` con i tre valori, preselezionato sul default di D1, e SHALL inviare il valore scelto in `POST /api/todos`.
- Il web e l'API SHALL ordinare l'elenco secondo la regola decisa in D2.
