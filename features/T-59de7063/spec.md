# T-59de7063 — Priorità dei todo

## Richiesta
Il team chiede di aggiungere una priorità ai todo (thread Teams della feature T-59de7063): nell'API un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione e modificabile con un endpoint dedicato, con filtro dell'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla alla creazione. Decisioni del team: default `media`, elenco ordinato per id, `PATCH /api/todos/{id}/priority`, 400 per valori non validi con confronto case-insensitive, filtro solo nell'API.

## Criteri di accettazione
- WHEN un client chiama `POST /api/todos` con un titolo valido e `priority` valida, l'API SHALL creare il todo con quella priorità (in minuscolo) e restituirla nella risposta.
- WHEN un client chiama `POST /api/todos` senza `priority`, l'API SHALL creare il todo con priorità `media`.
- IF `priority` non è uno tra `bassa`, `media`, `alta` (senza distinguere maiuscole/minuscole) THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority` e non creare né modificare il todo.
- WHEN un client chiama `PATCH /api/todos/{id}/priority` su un todo esistente con un valore valido, l'API SHALL aggiornare la priorità e restituire il todo aggiornato, lasciando invariati titolo e stato.
- IF `PATCH /api/todos/{id}/priority` riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con valore valido (case-insensitive), l'API SHALL restituire solo i todo con quella priorità, ordinati per id.
- IF `GET /api/todos?priority=<valore>` riceve un valore non valido THEN l'API SHALL rispondere 400.
- WHEN un client chiama `GET /api/todos` senza parametro `priority`, l'API SHALL restituire tutti i todo ordinati per id.
- WHEN un client chiama `GET /api/todos`, `GET /api/todos/{id}` o `POST /api/todos/{id}/done`, l'API SHALL includere `priority` nel todo restituito.
- WHEN il web mostra l'elenco, il web SHALL mostrare la priorità accanto a ogni todo come testo, con una classe di colore diversa per `bassa`, `media` e `alta`.
- WHERE il form di creazione è presente, il web SHALL offrire un menu con le tre priorità, con `media` preselezionata, e inviare quella scelta nella creazione.
- WHEN il web mostra l'elenco, il web SHALL ordinarlo senza tenere conto della priorità (regola per i todo fatti: D6).
- WHEN `dotnet test api` e `npm test --prefix web` vengono eseguiti, i test nuovi e quelli esistenti SHALL passare.
