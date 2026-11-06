# T-d1dfa20c — Priorità delle cose da fare

## Richiesta
Nel thread: "vorrei poter assegnare una priorità (alta/media/bassa) alle cose da fare, visibile e modificabile dal web".

## Criteri di accettazione
Valori assunti (da confermare, vedi D1 e D2 in plan.md): `alta`, `media`, `bassa`; predefinita `media`.

- WHEN l'API riceve `POST /api/todos` con `priority` valida, il sistema SHALL creare la cosa da fare con quella priorità.
- WHEN l'API riceve `POST /api/todos` senza `priority`, il sistema SHALL usare `media`.
- IF `priority` non è uno dei tre valori THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`.
- WHEN l'API riceve `POST /api/todos/{id}/priority` con `{ "priority": "..." }` valida, il sistema SHALL aggiornare la priorità e restituire la cosa da fare aggiornata.
- IF l'id non esiste THEN `POST /api/todos/{id}/priority` SHALL rispondere 404.
- Ogni cosa da fare restituita dall'API SHALL includere il campo `priority`.
- WHEN la pagina web mostra l'elenco, il sistema SHALL mostrare la priorità di ogni voce.
- WHEN l'utente sceglie una priorità diversa per una voce, la pagina SHALL chiamare l'API e ricaricare l'elenco.
- WHEN l'utente aggiunge una voce, la pagina SHALL permettere di scegliere la priorità (predefinita `media`).
- IF l'API risponde con errore THEN la pagina SHALL mostrare il messaggio nello spazio errori esistente.

## Fuori ambito (salvo risposta alle domande aperte)
Ordinamento per priorità, filtri, persistenza (i dati restano in memoria), modifica del titolo.
