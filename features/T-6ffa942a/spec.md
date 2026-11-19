# T-6ffa942a — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-6ffa942a (il link al thread non è arrivato nel messaggio del job):

> Vorrei aggiungere una priorità ai todo: nell'API un campo "priority" con valori bassa, media e alta, impostabile in creazione e modificabile con un endpoint dedicato, e la possibilità di filtrare l'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla quando si crea un todo. Non abbiamo deciso quale debba essere la priorità di default né l'ordine dell'elenco.

Risposta successiva nel thread: per tutte le domande aperte va bene la proposta del piano; dove manca, il bot sceglie la più semplice (vedi `plan.md`, D1–D7, e `decisions.md`).

I valori ammessi di `priority` sono `bassa`, `media`, `alta`: minuscoli, sensibili alle maiuscole. Il default è `media`.

## Criteri di accettazione

### API (`api`)
- Il servizio SHALL restituire in ogni todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done`, `POST /api/todos/{id}/priority`) il campo `priority` con uno tra `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `POST /api/todos` e un `priority` valido, il servizio SHALL salvarlo con quella priorità e restituirla nella risposta 201.
- WHEN un client crea un todo con `POST /api/todos` senza `priority`, il servizio SHALL assegnare `media`.
- IF `POST /api/todos` riceve un `priority` diverso da `bassa`, `media`, `alta` (comprese varianti maiuscole come `Alta`) THEN il servizio SHALL rispondere 400 con un errore di validazione sul campo `priority` e non creare il todo.
- WHEN un client chiama `POST /api/todos/{id}/priority` con corpo `{ "priority": "<valore valido>" }` su un todo esistente, il servizio SHALL aggiornare la priorità senza cambiare titolo e stato e restituire 200 con il todo aggiornato.
- IF `POST /api/todos/{id}/priority` riceve un `priority` assente o non valido THEN il servizio SHALL rispondere 400 e lasciare il todo invariato.
- IF `POST /api/todos/{id}/priority` riguarda un id inesistente THEN il servizio SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore valido>`, il servizio SHALL restituire solo i todo con quella priorità, ordinati per id.
- IF `GET /api/todos` riceve un `priority` non valido THEN il servizio SHALL rispondere 400.
- WHEN un client chiama `GET /api/todos` senza `priority`, il servizio SHALL restituire tutti i todo ordinati per id.
- Il `api/README.md` SHALL documentare il campo `priority`, il default, il filtro e l'endpoint `POST /api/todos/{id}/priority`.

### Web (`web`)
- Il web SHALL mostrare accanto a ogni todo la sua priorità come testo, con un colore diverso per ciascun valore (bassa verde, media arancio, alta rosso), in modo che il colore non sia l'unica informazione.
- Il form di creazione SHALL avere un menu (`<select>` con etichetta) con `bassa`, `media`, `alta`, preimpostato su `media`.
- WHEN l'utente aggiunge un todo, il web SHALL inviare a `POST /api/todos` la priorità scelta nel menu.
- IF la priorità passata ad `addTodo` non è tra `bassa`, `media`, `alta` THEN `web/src/todos.js` SHALL rifiutarla senza chiamare l'API.
- Il web SHALL ordinare l'elenco con prima le cose da fare, poi per priorità da `alta` a `bassa`, poi per id.
- Il web SHALL limitarsi a mostrare la priorità e a sceglierla in creazione: niente modifica della priorità né filtro nell'interfaccia.
- `web/tests/todos.test.js` SHALL coprire l'invio della priorità, il rifiuto di una priorità non valida e l'ordinamento.
