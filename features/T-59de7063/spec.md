# T-59de7063 — Priorità dei todo

## Richiesta
Il team chiede di aggiungere una priorità ai todo (thread Teams della feature T-59de7063): nell'API un campo `priority` con valori `bassa`, `media`, `alta`, impostabile in creazione e modificabile con un endpoint dedicato, con filtro dell'elenco per priorità; nel web la priorità accanto a ogni todo con un colore diverso e un menu per sceglierla alla creazione. Priorità di default e ordine dell'elenco non sono decisi (domande D1 e D2 in `plan.md`).

## Criteri di accettazione
- WHEN un client chiama `POST /api/todos` con un titolo valido e `priority` valida, l'API SHALL creare il todo con quella priorità e restituirla nella risposta.
- WHEN un client chiama `POST /api/todos` senza `priority`, l'API SHALL creare il todo con la priorità di default (da decidere, D1).
- IF `priority` non è uno tra `bassa`, `media`, `alta` THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority` e non creare né modificare il todo (D4).
- WHEN un client chiama l'endpoint dedicato per la priorità su un todo esistente con un valore valido, l'API SHALL aggiornare la priorità e restituire il todo aggiornato, lasciando invariati titolo e stato (forma dell'endpoint: D3).
- IF l'endpoint dedicato riceve un id inesistente THEN l'API SHALL rispondere 404.
- WHEN un client chiama `GET /api/todos?priority=<valore>` con valore valido, l'API SHALL restituire solo i todo con quella priorità.
- WHEN un client chiama `GET /api/todos` senza parametro `priority`, l'API SHALL restituire tutti i todo.
- WHEN un client chiama `GET /api/todos`, `GET /api/todos/{id}` o `POST /api/todos/{id}/done`, l'API SHALL includere `priority` nel todo restituito.
- WHEN il web mostra l'elenco, il web SHALL mostrare la priorità accanto a ogni todo come testo, con una classe di colore diversa per `bassa`, `media` e `alta`.
- WHERE il form di creazione è presente, il web SHALL offrire un menu con le tre priorità e inviare quella scelta nella creazione (preselezione secondo D1).
- WHEN il web mostra l'elenco, il web SHALL ordinarlo secondo la regola decisa (D2).
- WHEN `dotnet test api` e `npm test --prefix web` vengono eseguiti, i test nuovi e quelli esistenti SHALL passare.
