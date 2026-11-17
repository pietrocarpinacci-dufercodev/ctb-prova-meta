# T-dd485a10 — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-dd485a10: aggiungere una priorità ai todo.
- API: campo `priority` con valori bassa, media e alta, impostabile in creazione, modificabile con un endpoint dedicato, e filtro dell'elenco per priorità.
- Web: priorità mostrata accanto a ogni todo con un colore diverso per valore e un menu per sceglierla quando si crea un todo.
- Default e ordine dell'elenco, non decisi all'inizio, sono stati fissati nel thread accettando le proposte del piano (D1–D6 in `plan.md`).

## Criteri di accettazione
Valori ammessi nel JSON: `bassa`, `media`, `alta`, minuscoli, con confronto esatto (D3). Default: `media` (D1).

- Il sistema (API) SHALL restituire in ogni todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos`, `POST /api/todos/{id}/done`, `PUT /api/todos/{id}/priority`) il campo `priority` con uno dei tre valori ammessi.
- WHEN un client crea un todo con `POST /api/todos` e un `priority` ammesso, l'API SHALL salvare e restituire quella priorità con risposta 201.
- WHEN un client crea un todo senza `priority`, l'API SHALL assegnare `media`.
- IF `POST /api/todos` riceve un `priority` non ammesso (compresi valori con maiuscole, es. `Alta`) THEN l'API SHALL rispondere 400 con un ValidationProblem sulla chiave `priority`, senza creare il todo.
- WHEN un client invia `PUT /api/todos/{id}/priority` con body `{ "priority": "<valore ammesso>" }` per un todo esistente, l'API SHALL aggiornare la priorità e restituire il todo aggiornato con risposta 200, senza cambiare titolo e stato `done`.
- IF `PUT /api/todos/{id}/priority` riceve un id inesistente THEN l'API SHALL rispondere 404.
- IF `PUT /api/todos/{id}/priority` riceve una priorità non ammessa o assente THEN l'API SHALL rispondere 400 con un ValidationProblem sulla chiave `priority`.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con un solo valore ammesso, l'API SHALL restituire solo i todo con quella priorità.
- WHEN un client chiama `GET /api/todos` senza `priority`, l'API SHALL restituire tutti i todo come oggi.
- IF `GET /api/todos` riceve un `priority` non ammesso THEN l'API SHALL rispondere 400 con un ValidationProblem sulla chiave `priority`.
- L'API SHALL mantenere l'ordine attuale dell'elenco (per id) e il web SHALL mantenere l'ordine attuale di `sortTodos` (prima i non fatti, poi per id): la priorità non cambia l'ordine (D2).
- Il sistema (web) SHALL mostrare la priorità accanto a ogni todo come testo, con un colore diverso per ciascuno dei tre valori: `bassa` verde/grigio, `media` ambra, `alta` rosso, con contrasto sufficiente (il colore non è l'unica indicazione).
- WHERE il form di creazione è presente, il web SHALL offrire un menu `<select>` con etichetta accessibile e i tre valori, preselezionato su `media`, e SHALL inviare il valore scelto in `POST /api/todos`.
- Il web SHALL solo mostrare la priorità e permettere di sceglierla in creazione: niente modifica della priorità né filtro nella pagina (D6).
