# T-d1dfa20c — Priorità delle cose da fare

## Richiesta
Nel thread: "vorrei poter assegnare una priorità (alta/media/bassa) alle cose da fare, visibile e modificabile dal web". Risposte: valori `high`, `medium`, `low` (D1); priorità in `POST /api/todos` e `PATCH /api/todos/{id}` (D2); obbligatoria in POST, senza predefinito (D3); PATCH parziale con gli stessi campi del POST (D4); ordinamento per priorità (D5); modificabile anche sulle voci fatte (D6).

## Criteri di accettazione
Valori: `high`, `medium`, `low`. Le assunzioni ancora aperte sono in plan.md (D7, D8).

- WHEN l'API riceve `POST /api/todos` con `title` e `priority` valide, il sistema SHALL creare la cosa da fare con quella priorità.
- IF `priority` manca o non è uno dei tre valori in `POST /api/todos` THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`, senza creare nulla.
- WHEN l'API riceve `PATCH /api/todos/{id}` con alcuni dei campi `title`, `priority`, il sistema SHALL aggiornare solo i campi presenti e restituire la cosa da fare aggiornata.
- WHEN un campo non è presente nel corpo del PATCH, il sistema SHALL lasciarlo invariato.
- WHERE un campo ammette `null`, WHEN il PATCH lo contiene con valore `null`, il sistema SHALL metterlo a `null` (nessun campo lo ammette oggi, vedi D7).
- IF un valore del PATCH non è valido (`priority` fuori dai tre valori, `title` vuoto) THEN l'API SHALL rispondere 400 senza modificare nulla.
- WHEN la cosa da fare è già fatta, il PATCH SHALL poter cambiarne la priorità.
- IF l'id non esiste THEN `PATCH /api/todos/{id}` SHALL rispondere 404.
- Ogni cosa da fare restituita dall'API SHALL includere il campo `priority`.
- WHEN la pagina web mostra l'elenco, il sistema SHALL mostrare la priorità di ogni voce, anche per quelle fatte.
- WHEN l'utente sceglie una priorità diversa per una voce, la pagina SHALL chiamare `PATCH` e ricaricare l'elenco.
- WHEN l'utente aggiunge una voce, la pagina SHALL richiedere la scelta della priorità, senza valore preselezionato.
- WHEN la pagina mostra l'elenco, il sistema SHALL ordinarlo per stato (non fatte prima), poi priorità `high`→`medium`→`low`, poi id (da confermare, D8).
- IF l'API risponde con errore THEN la pagina SHALL mostrare il messaggio nello spazio errori esistente.

## Fuori ambito
Filtri, persistenza (i dati restano in memoria), modifica di `done` via PATCH (resta `POST /api/todos/{id}/done`, perché `done` non è nel POST).
