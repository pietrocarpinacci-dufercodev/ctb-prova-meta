# T-625c8b7d — Priorità delle cose da fare

## Richiesta
Dal thread Teams della feature T-625c8b7d: aggiungere una priorità ai todo. Nell'API un campo `priority` con valori bassa, media e alta, impostabile in creazione e modificabile con un endpoint dedicato, e la possibilità di filtrare l'elenco per priorità. Nel web la priorità accanto a ogni todo, con un colore diverso per valore, e un menu per sceglierla quando si crea un todo. Il team non ha ancora deciso la priorità di default né l'ordine dell'elenco (vedi domande aperte in `plan.md`).

## Criteri di accettazione
I valori di `priority` nel JSON, il default, la forma dell'endpoint, il filtro e l'ordine dipendono dalle domande D1-D5 di `plan.md`: qui c'è la proposta del piano.

- Il sistema API SHALL restituire in ogni todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done`) il campo `priority` con uno dei valori `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `POST /api/todos` e un campo `priority` valido, il sistema API SHALL salvare il todo con quella priorità e restituirla nella risposta 201.
- WHEN un client crea un todo con `POST /api/todos` senza il campo `priority`, il sistema API SHALL assegnare la priorità di default decisa in D1.
- IF il campo `priority` in creazione ha un valore diverso da `bassa`, `media`, `alta` THEN il sistema API SHALL rispondere 400 con un errore di validazione sul campo `priority` e non creare il todo.
- WHEN un client chiama `PUT /api/todos/{id}/priority` con `{ "priority": "<valore valido>" }`, il sistema API SHALL aggiornare la priorità del todo e restituire 200 con il todo aggiornato.
- IF il todo indicato in `PUT /api/todos/{id}/priority` non esiste THEN il sistema API SHALL rispondere 404.
- IF il valore di `priority` in `PUT /api/todos/{id}/priority` è mancante o non valido THEN il sistema API SHALL rispondere 400 e lasciare invariato il todo.
- WHEN un client chiama `GET /api/todos?priority=<valore valido>`, il sistema API SHALL restituire solo i todo con quella priorità.
- IF il parametro `priority` di `GET /api/todos` ha un valore non valido THEN il sistema API SHALL rispondere 400.
- WHEN un client chiama `GET /api/todos` senza parametro `priority`, il sistema API SHALL restituire tutti i todo, come oggi.
- Il sistema web SHALL mostrare accanto al titolo di ogni todo la sua priorità come testo, con un colore diverso per ciascuno dei tre valori.
- Il sistema web SHALL offrire nel modulo di creazione un menu a tendina con le tre priorità, preselezionato sul default deciso in D1, e inviare il valore scelto nel campo `priority` di `POST /api/todos`.
- IF il valore di priorità passato a `addTodo` non è uno dei tre ammessi THEN il sistema web SHALL rifiutarlo senza chiamare l'API.
- Il sistema web SHALL ordinare l'elenco secondo la regola decisa in D2.
