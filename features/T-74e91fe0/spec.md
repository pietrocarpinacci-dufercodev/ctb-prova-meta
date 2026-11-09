# T-74e91fe0 — Priorità dei todo

## Richiesta
Il team chiede nel thread Teams (T-74e91fe0) di aggiungere una priorità ai todo: nell'API un campo `priority` (`bassa`, `media`, `alta`) impostabile in creazione, modificabile con un endpoint dedicato e usabile per filtrare l'elenco; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla alla creazione. Priorità di default e ordine sono stati decisi nel thread (vedi `decisions.md`). Fuori ambito: modifica della priorità e filtro nel web.

## Criteri di accettazione
- WHEN un client chiede un todo (singolo o elenco), l'API SHALL includere il campo `priority` con uno dei valori `bassa`, `media`, `alta`.
- WHEN un client crea un todo con `priority` valida, l'API SHALL salvarla e restituirla nella risposta 201.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità di default `media`.
- IF la `priority` in creazione non è uno dei tre valori THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority` senza creare il todo.
- WHEN un client chiama `PUT /api/todos/{id}/priority` con `{ "priority": "<valore>" }` per un todo esistente con una priorità valida, l'API SHALL aggiornarla e restituire il todo aggiornato.
- IF l'endpoint dedicato riceve una priorità non valida THEN l'API SHALL rispondere 400; IF il todo non esiste THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>`, l'API SHALL restituire solo i todo con quella priorità.
- IF il filtro `priority` ha un valore non valido THEN l'API SHALL rispondere 400.
- WHEN l'API riceve una priorità in ingresso, essa SHALL accettarla senza distinzione di maiuscole e restituirla in minuscolo.
- WHEN `GET /api/todos` è chiamato senza filtro, l'API SHALL restituire tutti i todo in ordine di id.
- WHEN il web mostra l'elenco, esso SHALL ordinare i todo non fatti prima, poi per priorità da alta a bassa, poi per id.
- WHEN il web mostra un todo, esso SHALL mostrare la priorità accanto al titolo con un colore diverso per ciascuno dei tre valori.
- WHEN l'utente crea un todo dal web, il web SHALL offrire un menu con le tre priorità, preselezionato su `media`, e inviare il valore scelto.
- IF l'API restituisce un todo senza `priority` THEN il web SHALL mostrare la priorità di default, senza errori.
- WHERE il contratto API cambia, `api/README.md`, `web/src/todos.js` e i test di entrambi i submodule SHALL essere allineati.
