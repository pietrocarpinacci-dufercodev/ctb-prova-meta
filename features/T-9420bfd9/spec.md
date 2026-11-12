# T-9420bfd9 — Priorità dei todo

## Richiesta
Dal thread Teams della feature T-9420bfd9 (nel job non c'era il link al thread): aggiungere una priorità ai todo.
- API: un campo `priority` con valori `bassa`, `media` e `alta`, da impostare quando si crea un todo, da modificare con un endpoint dedicato, e un filtro per priorità sull'elenco.
- Web: la priorità accanto a ogni todo, con un colore diverso per valore, e un menu per sceglierla quando si crea un todo.
- Nel thread sono state accettate tutte le proposte del piano (D1–D8): default `media`, ordine dell'elenco, formato, endpoint, filtro, ambito del web, agente e colori.

## Criteri di accettazione
API (`api`)
- Il sistema SHALL restituire il campo `priority`, con valore `"bassa"`, `"media"` o `"alta"`, in ogni todo delle risposte di questi endpoint:
  - `GET /api/todos`
  - `GET /api/todos/{id}`
  - `POST /api/todos`
  - `POST /api/todos/{id}/done`
  - `PUT /api/todos/{id}/priority`
- WHEN si chiama `POST /api/todos` con `priority` valida, il sistema SHALL creare il todo con quella priorità.
- WHEN si chiama `POST /api/todos` senza `priority` (assente o `null`), il sistema SHALL creare il todo con priorità `media`.
- IF `POST /api/todos` riceve una `priority` diversa dalle stringhe minuscole `bassa`, `media`, `alta` (comprese varianti con maiuscole) THEN il sistema SHALL rispondere 400 con un errore di validazione sul campo `priority` e SHALL non creare il todo.
- WHEN si chiama `PUT /api/todos/{id}/priority` con body `{ "priority": "<valore valido>" }` su un todo esistente, il sistema SHALL aggiornare la priorità e rispondere 200 con il todo aggiornato, lasciando invariati titolo e stato.
- IF `PUT /api/todos/{id}/priority` riceve una priorità assente o non valida THEN il sistema SHALL rispondere 400 e SHALL lasciare il todo invariato.
- IF `PUT /api/todos/{id}/priority` riguarda un id inesistente THEN il sistema SHALL rispondere 404.
- WHEN si chiama `GET /api/todos?priority=<valore valido>`, il sistema SHALL restituire solo i todo con quella priorità.
- WHEN si chiama `GET /api/todos` senza il parametro `priority`, il sistema SHALL restituire tutti i todo, come oggi.
- IF `GET /api/todos` riceve un parametro `priority` non valido THEN il sistema SHALL rispondere 400.
- Il sistema SHALL restituire l'elenco ordinato per id, come oggi.

Web (`web`)
- Il modulo di creazione SHALL avere un menu (`<select>` con etichetta) con le opzioni bassa, media e alta, preselezionato su `media`.
- WHEN l'utente aggiunge un todo, il web SHALL mandare all'API la priorità scelta nel menu.
- Il web SHALL mostrare accanto al titolo di ogni todo la priorità come testo, colorata così:
  - verde per bassa;
  - ambra per media;
  - rosso per alta.
- Il testo della priorità SHALL avere un contrasto di almeno 4.5:1 sul fondo della pagina.
- IF un todo arriva dall'API senza `priority` o con un valore sconosciuto THEN il web SHALL mostrarlo senza errori e senza colore di priorità.
- Il web SHALL ordinare l'elenco così:
  - prima i todo non fatti;
  - poi per priorità, da alta a bassa, con i valori sconosciuti per ultimi;
  - poi per id.
- Il web SHALL non offrire filtro né modifica della priorità dei todo esistenti (fuori ambito).

Contratto e documentazione
- WHEN cambia il contratto, questi file SHALL essere aggiornati nella stessa feature:
  - `api/README.md`;
  - l'elenco degli endpoint in `CLAUDE.md` del meta-repo;
  - `web/src/todos.js`;
  - i test di entrambi i submodule.
