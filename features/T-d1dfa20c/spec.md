# T-d1dfa20c — Priorità delle cose da fare

## Richiesta
Nel thread: "vorrei poter assegnare una priorità (alta/media/bassa) alle cose da fare, visibile e modificabile dal web". Risposte: valori `high`, `medium`, `low` (D1); priorità in `POST /api/todos` e `PATCH /api/todos/{id}` (D2); presente in POST, senza predefinito (D3), anche `null` (D9); PATCH parziale con gli stessi campi del POST (D4); ordinamento per priorità (D5); modificabile anche sulle voci fatte (D6); rimovibile, quindi nullable dopo la creazione (D7); ordine non fatte, `high`→`medium`→`low`→null, id (D8).

## Criteri di accettazione
Valori: `high`, `medium`, `low`, oppure `null` (nessuna priorità).

- WHEN l'API riceve `POST /api/todos` con `title` e `priority` valide, il sistema SHALL creare la cosa da fare con quella priorità.
- WHEN l'API riceve `POST /api/todos` con `"priority": null`, il sistema SHALL creare la cosa da fare con `priority` `null`.
- IF `priority` è presente ma non è `high`, `medium`, `low` né `null` in `POST /api/todos` THEN l'API SHALL rispondere 400 con errore di validazione sul campo `priority`, senza creare nulla.
- IF la proprietà `priority` è assente in `POST /api/todos` THEN l'API SHALL rispondere 400 (nessun valore predefinito; da confermare, D10).
- WHEN l'API riceve `PATCH /api/todos/{id}` con alcuni dei campi `title`, `priority`, il sistema SHALL aggiornare solo i campi presenti e restituire la cosa da fare aggiornata.
- WHEN un campo non è presente nel corpo del PATCH, il sistema SHALL lasciarlo invariato.
- WHEN il PATCH contiene `"priority": null`, il sistema SHALL rimuovere la priorità (valore `null`).
- IF il PATCH contiene `"title": null` o un titolo vuoto THEN l'API SHALL rispondere 400 senza modificare nulla.
- IF `priority` nel PATCH non è `high`, `medium`, `low` né `null` THEN l'API SHALL rispondere 400 senza modificare nulla.
- WHEN la cosa da fare è già fatta, il PATCH SHALL poter cambiarne la priorità.
- IF l'id non esiste THEN `PATCH /api/todos/{id}` SHALL rispondere 404.
- Ogni cosa da fare restituita dall'API SHALL includere il campo `priority` (anche `null`).
- WHEN la pagina mostra l'elenco, il sistema SHALL mostrare la priorità di ogni voce, anche fatta, e "nessuna priorità" quando è `null`.
- WHEN l'utente sceglie una priorità diversa (o "nessuna") per una voce, la pagina SHALL chiamare `PATCH` (con `null` per "nessuna") e ricaricare l'elenco.
- WHEN l'utente aggiunge una voce, la pagina SHALL richiedere la scelta della priorità, senza valore preselezionato.
- WHEN la pagina mostra l'elenco, il sistema SHALL ordinarlo: non fatte prima, poi `high`, `medium`, `low`, `null`, poi id (stesso criterio dentro il gruppo delle fatte).
- IF l'API risponde con errore THEN la pagina SHALL mostrare il messaggio nello spazio errori esistente.

## Fuori ambito
Filtri, persistenza (i dati restano in memoria), modifica di `done` via PATCH (resta `POST /api/todos/{id}/done`, perché `done` non è nel POST).
