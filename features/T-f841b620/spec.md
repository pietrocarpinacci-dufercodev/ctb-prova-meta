# T-f841b620 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams della feature T-f841b620 di aggiungere una priorità ai todo:
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.

Il team non ha ancora deciso la priorità di default né l'ordine dell'elenco (domande D1 e D2 in `plan.md`).

## Criteri di accettazione
I valori JSON `low|medium|high`, la forma dell'endpoint e il 400 sui valori non validi sono proposte del piano (D3, D4, D5): si confermano con l'approvazione.

- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `low`, `medium`, `high` (bassa, media, alta).
- WHEN arriva `POST /api/todos` con `priority` valido, l'API SHALL creare il todo con quella priorità.
- WHEN arriva `POST /api/todos` senza `priority`, l'API SHALL creare il todo con la priorità di default decisa in D1.
- IF `POST /api/todos` contiene un `priority` non tra i valori ammessi THEN l'API SHALL rispondere 400 con un problema di validazione sul campo `priority`.
- WHEN arriva `PUT /api/todos/{id}/priority` con `{ "priority": "<valore>" }` valido, l'API SHALL aggiornare la priorità e restituire 200 con il todo aggiornato.
- IF `PUT /api/todos/{id}/priority` riguarda un id inesistente THEN l'API SHALL rispondere 404.
- IF `PUT /api/todos/{id}/priority` contiene un valore mancante o non ammesso THEN l'API SHALL rispondere 400.
- WHEN arriva `GET /api/todos?priority=<valore>` con valore ammesso, l'API SHALL restituire solo i todo con quella priorità.
- WHEN arriva `GET /api/todos` senza `priority`, l'API SHALL restituire tutti i todo come oggi.
- IF `GET /api/todos?priority=<valore>` ha un valore non ammesso THEN l'API SHALL rispondere 400.
- L'elenco (API e web) SHALL essere ordinato secondo la regola decisa in D2.
- Il web SHALL mostrare accanto a ogni todo la priorità come testo (Bassa, Media, Alta) con un colore diverso per ciascun valore, così che l'informazione non dipenda solo dal colore.
- Il form di creazione del web SHALL avere un menu `<select>` con etichetta per scegliere la priorità, preselezionato sul default deciso in D1.
- WHEN si crea un todo dal web, `addTodo` SHALL inviare all'API la priorità scelta nel menu.
- `api/README.md`, `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati al nuovo contratto.
