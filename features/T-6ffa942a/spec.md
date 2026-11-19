# T-6ffa942a — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-6ffa942a (il link al thread non è arrivato nel messaggio del job):

> Vorrei aggiungere una priorità ai todo: nell'API un campo "priority" con valori bassa, media e alta, impostabile in creazione e modificabile con un endpoint dedicato, e la possibilità di filtrare l'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla quando si crea un todo. Non abbiamo deciso quale debba essere la priorità di default né l'ordine dell'elenco.

Nei criteri qui sotto `<valori>` indica i tre valori ammessi (bassa/media/alta oppure low/medium/high, vedi D3 in `plan.md`), `<default>` la priorità di default (D1) e `<endpoint priorità>` l'endpoint dedicato (D4). Vanno fissati prima dell'approvazione.

## Criteri di accettazione

### API (`api`)
- Il servizio SHALL restituire in ogni todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done`) il campo `priority` con uno dei `<valori>`.
- WHEN un client crea un todo con `POST /api/todos` e un `priority` valido, il servizio SHALL salvarlo con quella priorità e restituirla nella risposta 201.
- WHEN un client crea un todo con `POST /api/todos` senza `priority`, il servizio SHALL assegnare `<default>`.
- IF `POST /api/todos` riceve un `priority` diverso dai `<valori>` THEN il servizio SHALL rispondere 400 con un errore di validazione sul campo `priority` e non creare il todo.
- WHEN un client chiama `<endpoint priorità>` su un todo esistente con un `priority` valido, il servizio SHALL aggiornare la priorità senza cambiare titolo e stato e restituire 200 con il todo aggiornato.
- IF `<endpoint priorità>` riceve un `priority` assente o non valido THEN il servizio SHALL rispondere 400 e lasciare il todo invariato.
- IF `<endpoint priorità>` riguarda un id inesistente THEN il servizio SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con un valore valido, il servizio SHALL restituire solo i todo con quella priorità.
- IF `GET /api/todos` riceve un `priority` non valido THEN il servizio SHALL rispondere 400.
- WHEN un client chiama `GET /api/todos` senza `priority`, il servizio SHALL restituire tutti i todo, nell'ordine deciso in D2.
- Il `api/README.md` SHALL documentare il campo `priority`, il filtro e il nuovo endpoint.

### Web (`web`)
- Il web SHALL mostrare accanto a ogni todo la sua priorità come testo, con un colore diverso per ciascuno dei tre valori (il colore non è l'unica informazione).
- Il form di creazione SHALL avere un menu (`<select>` con etichetta) con le tre priorità, preimpostato su `<default>`.
- WHEN l'utente aggiunge un todo, il web SHALL inviare a `POST /api/todos` la priorità scelta nel menu.
- IF la priorità passata ad `addTodo` non è tra i `<valori>` THEN `web/src/todos.js` SHALL rifiutarla senza chiamare l'API.
- Il web SHALL ordinare l'elenco secondo D2.
- `web/tests/todos.test.js` SHALL coprire l'invio della priorità, il rifiuto di una priorità non valida e l'ordinamento.
