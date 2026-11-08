# T-5164e6ed — Scadenza dei todo

## Richiesta
Il team chiede (thread Teams del job T-5164e6ed) di aggiungere una scadenza ai todo: nell'API un campo facoltativo `dueDate` (solo data) in creazione e in lettura, e un endpoint per elencare i todo scaduti; nel web la data accanto a ogni todo, evidenziata se è passata. Risposte del team: i completati non sono scaduti e non vanno evidenziati; "oggi" è la data UTC del server per l'API e quella locale del browser per il web; un todo che scade oggi non è scaduto; l'endpoint è `GET /api/todos/overdue`; il form del web ha un campo data facoltativo.

## Criteri di accettazione
- WHEN un client invia `POST /api/todos` con `dueDate` nel formato `yyyy-MM-dd`, l'API SHALL rispondere 201 con il todo che contiene lo stesso `dueDate`.
- WHEN un client invia `POST /api/todos` senza `dueDate`, l'API SHALL creare il todo con `dueDate` nullo, come oggi.
- WHEN un client legge un todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos/{id}/done`), l'API SHALL includere `dueDate` (data `yyyy-MM-dd` oppure `null`).
- IF `dueDate` non è una data valida `yyyy-MM-dd` THEN l'API SHALL rispondere 400 e non creare il todo.
- WHEN un client chiama `GET /api/todos/overdue`, l'API SHALL elencare i todo non completati con `dueDate` precedente alla data UTC di oggi, ordinati per `dueDate` crescente e, a parità di data, per id.
- WHERE un todo non ha `dueDate`, l'API SHALL escluderlo da `GET /api/todos/overdue`.
- WHERE un todo è completato, l'API SHALL escluderlo da `GET /api/todos/overdue`.
- WHEN un todo ha `dueDate` uguale alla data UTC di oggi, l'API SHALL non considerarlo scaduto.
- WHEN il web mostra un todo con `dueDate`, la pagina SHALL mostrare la data accanto al titolo.
- WHEN il web mostra un todo non completato con `dueDate` precedente alla data locale del browser, la pagina SHALL evidenziare la data.
- WHERE un todo è completato, la pagina SHALL mostrare la data senza evidenziarla.
- WHERE un todo non ha `dueDate`, la pagina SHALL mostrare solo il titolo.
- WHEN l'utente invia il form con una data nel campo facoltativo, il web SHALL creare il todo con quel `dueDate`.
- WHEN l'utente invia il form con il campo data vuoto, il web SHALL creare il todo senza inviare `dueDate`.
- WHEN i todo vengono creati senza `dueDate` (client esistenti), il comportamento esistente di API e web SHALL restare invariato.
