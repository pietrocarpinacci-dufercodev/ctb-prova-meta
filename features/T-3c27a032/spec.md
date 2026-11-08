# T-3c27a032 — Priorità dei todo

## Richiesta
Il team chiede nel thread Teams (T-3c27a032) di aggiungere una priorità ai todo: nell'API un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione e modificabile con un endpoint dedicato, con filtro dell'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla alla creazione. Priorità di default e ordine dell'elenco non sono ancora decisi (vedi Domande aperte in `plan.md`).

## Criteri di accettazione
- Il todo restituito dall'API SHALL contenere il campo `priority` con uno dei valori `bassa`, `media`, `alta`.
- WHEN `POST /api/todos` riceve un `priority` valido, l'API SHALL creare il todo con quella priorità.
- WHEN `POST /api/todos` non riceve `priority`, l'API SHALL usare la priorità di default (da decidere, D1).
- IF `priority` in creazione o nell'endpoint dedicato non è uno dei tre valori THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`.
- WHEN viene chiamato l'endpoint dedicato di modifica (forma da decidere, D3) con una priorità valida su un todo esistente, l'API SHALL aggiornare la priorità e restituire il todo aggiornato.
- IF l'endpoint di modifica riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN `GET /api/todos?priority=<valore>` è chiamato con un valore valido, l'API SHALL restituire solo i todo con quella priorità.
- IF `GET /api/todos?priority=<valore>` ha un valore non valido THEN l'API SHALL rispondere 400.
- WHEN `GET /api/todos` è chiamato senza filtro, l'API SHALL restituire tutti i todo, nell'ordine deciso in D2.
- Il web SHALL mostrare accanto a ogni todo la sua priorità con un colore diverso per ciascun valore.
- WHEN l'utente crea un todo dal web, il web SHALL inviare la priorità scelta nel menu (valore iniziale del menu = default, D1).
- WHERE il web usa `sortTodos`, l'ordine SHALL rispettare la decisione D2.
- Test aggiornati in `api/tests` e `web` per ogni criterio sopra; contratto riportato in `api/README.md` e `web/src/todos.js`.
