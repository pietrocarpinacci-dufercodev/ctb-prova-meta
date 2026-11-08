# T-d96c0059 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams (feature T-d96c0059) di aggiungere una priorità ai todo: nell'API un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione e modificabile con un endpoint dedicato, con filtro dell'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla alla creazione. Priorità di default e ordine dell'elenco non sono ancora decisi (vedi Domande aperte in `plan.md`).

## Criteri di accettazione
- WHEN un client crea un todo con `priority` valido, l'API SHALL salvarlo e restituirlo con quel valore.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare la priorità di default (da decidere, D1).
- IF `priority` non è uno tra `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`.
- WHEN un client chiama l'endpoint dedicato di modifica con un valore valido su un todo esistente, l'API SHALL aggiornare la priorità e restituire il todo aggiornato.
- IF il todo indicato nell'endpoint di modifica non esiste THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>`, l'API SHALL restituire solo i todo con quella priorità.
- IF il filtro `priority` ha un valore non valido THEN l'API SHALL rispondere 400.
- WHEN il web mostra un todo, il web SHALL mostrare la sua priorità accanto al titolo con un colore diverso per ciascun valore.
- WHEN l'utente crea un todo dal web, il web SHALL inviare la priorità scelta nel menu.
- WHERE l'ordine dell'elenco dipende dalla priorità, il web e l'API SHALL applicare l'ordine deciso (D2).
