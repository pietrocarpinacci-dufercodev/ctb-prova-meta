# T-f841b620 — Priorità dei todo

## Richiesta
Il team ha chiesto nel thread Teams della feature T-f841b620 di aggiungere una priorità ai todo:
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità;
- web: priorità mostrata accanto a ogni todo con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.

Priorità di default e ordine dell'elenco, inizialmente non decisi, sono stati fissati nel thread accettando le proposte del piano (D1–D7 in `plan.md`, riportate in `decisions.md`).

## Criteri di accettazione
- Ogni todo restituito dall'API SHALL avere il campo `priority` con uno dei valori `low`, `medium`, `high` (bassa, media, alta) (D3).
- WHEN arriva `POST /api/todos` con `priority` valido, l'API SHALL creare il todo con quella priorità.
- WHEN arriva `POST /api/todos` senza `priority`, l'API SHALL creare il todo con priorità `medium` (D1).
- IF `POST /api/todos` contiene un `priority` non tra i valori ammessi THEN l'API SHALL rispondere 400 con un problema di validazione sul campo `priority` (D5).
- WHEN arriva `PUT /api/todos/{id}/priority` con `{ "priority": "<valore>" }` valido, l'API SHALL aggiornare la priorità e restituire 200 con il todo aggiornato (D4).
- IF `PUT /api/todos/{id}/priority` riguarda un id inesistente THEN l'API SHALL rispondere 404.
- IF `PUT /api/todos/{id}/priority` contiene un valore mancante o non ammesso THEN l'API SHALL rispondere 400 (D5).
- WHEN arriva `GET /api/todos?priority=<valore>` con valore ammesso, l'API SHALL restituire solo i todo con quella priorità.
- WHEN arriva `GET /api/todos` senza `priority`, l'API SHALL restituire tutti i todo ordinati per id, come oggi (D2).
- IF `GET /api/todos?priority=<valore>` ha un valore non ammesso THEN l'API SHALL rispondere 400 (D5).
- Il web SHALL ordinare l'elenco con i todo non fatti prima, poi per priorità da alta a bassa, poi per id (D2).
- Il web SHALL mostrare accanto a ogni todo la priorità come testo (Bassa, Media, Alta) con un colore diverso per ciascun valore, così che l'informazione non dipenda solo dal colore.
- Il form di creazione del web SHALL avere un menu `<select>` con etichetta per scegliere la priorità, con Media preselezionata (D1).
- WHEN si crea un todo dal web, `addTodo` SHALL inviare all'API la priorità scelta nel menu.
- Il web SHALL NOT offrire filtro per priorità né modifica della priorità di un todo esistente (D6).
- `api/README.md`, `web/src/todos.js` e i test di entrambi i submodule SHALL essere aggiornati al nuovo contratto.
