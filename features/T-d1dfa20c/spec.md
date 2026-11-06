# T-d1dfa20c — Priorità delle cose da fare

## Richiesta
Nel thread: "vorrei poter assegnare una priorità (alta/media/bassa) alle cose da fare, visibile e modificabile dal web". Risposte: valori in inglese `high`, `medium`, `low` (D1); priorità sia in `POST /api/todos` sia in `PATCH /api/todos/{id}` (D2).

## Criteri di accettazione
Valori: `high`, `medium`, `low`. Il predefinito `medium` è un'assunzione (D3 in plan.md).

- WHEN l'API riceve `POST /api/todos` con `priority` valida, il sistema SHALL creare la cosa da fare con quella priorità.
- WHEN l'API riceve `POST /api/todos` senza `priority`, il sistema SHALL usare `medium`.
- WHEN l'API riceve `PATCH /api/todos/{id}` con `{ "priority": "..." }` valida, il sistema SHALL aggiornare la priorità e restituire la cosa da fare aggiornata.
- IF `priority` non è uno dei tre valori, in POST o in PATCH, THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`.
- IF il corpo del PATCH non contiene `priority` THEN l'API SHALL rispondere 400.
- IF l'id non esiste THEN `PATCH /api/todos/{id}` SHALL rispondere 404.
- Ogni cosa da fare restituita dall'API SHALL includere il campo `priority`.
- WHEN la pagina web mostra l'elenco, il sistema SHALL mostrare la priorità di ogni voce.
- WHEN l'utente sceglie una priorità diversa per una voce, la pagina SHALL chiamare `PATCH` e ricaricare l'elenco.
- WHEN l'utente aggiunge una voce, la pagina SHALL permettere di scegliere la priorità (predefinita `medium`).
- IF l'API risponde con errore THEN la pagina SHALL mostrare il messaggio nello spazio errori esistente.

## Fuori ambito (salvo risposta alle domande aperte)
Ordinamento per priorità, filtri, persistenza (i dati restano in memoria), modifica di titolo o stato `done` via PATCH.
