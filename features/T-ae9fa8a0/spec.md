# T-ae9fa8a0 — Priorità dei todo

## Richiesta
Il team chiede di aggiungere una priorità ai todo (thread Teams della feature T-ae9fa8a0):
- API: campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione, modificabile con un endpoint dedicato, filtro dell'elenco per priorità.
- Web: priorità accanto a ogni todo con colore diverso per valore e menu di scelta alla creazione.
- Priorità di default e ordine dell'elenco non sono decisi: vedi domande aperte D1 e D2 in `plan.md`. I criteri sotto li lasciano parametrici (`<DEFAULT>`, `<ORDINE>`) finché non c'è risposta.

## Criteri di accettazione
- WHEN `POST /api/todos` riceve `priority` valida (`bassa`, `media` o `alta`), l'API SHALL creare il todo con quella priorità e restituirla nella risposta.
- WHEN `POST /api/todos` non riceve `priority`, l'API SHALL creare il todo con la priorità `<DEFAULT>` (D1).
- IF `priority` non è uno dei tre valori ammessi THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`, senza creare o modificare il todo.
- WHEN arriva la richiesta all'endpoint dedicato di modifica priorità (percorso e verbo in D3) con valore valido su un todo esistente, l'API SHALL aggiornare solo la priorità e restituire il todo aggiornato.
- IF l'endpoint di modifica priorità riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN `GET /api/todos?priority=<valore>` riceve un valore valido, l'API SHALL restituire solo i todo con quella priorità.
- IF `priority` nella query non è un valore ammesso THEN l'API SHALL rispondere 400.
- WHEN `GET /api/todos` non ha il parametro `priority`, l'API SHALL restituire tutti i todo nell'ordine `<ORDINE>` (D2).
- WHEN `GET /api/todos/{id}` o `POST /api/todos/{id}/done` restituiscono un todo, la risposta SHALL includere `priority`.
- WHEN il web mostra un todo, il web SHALL mostrare la sua priorità accanto al titolo con un colore diverso per `bassa`, `media` e `alta`.
- WHEN l'utente crea un todo dal web, il web SHALL offrire un menu con le tre priorità, preselezionato su `<DEFAULT>`, e inviare il valore scelto.
- IF il server restituisce un todo senza `priority` o con valore sconosciuto THEN il web SHALL mostrarlo senza errori, con lo stile neutro.
- WHEN il contratto cambia, i test di `api` e `web` SHALL coprire i casi sopra e `npm test --prefix web` e `dotnet test api` SHALL passare.
