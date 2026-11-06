# T-729d0695 — DELETE /api/todos/{id} con 200 e elemento rimosso

## Richiesta
Nel thread: "cambia DELETE /api/todos/{id} perché risponda 200 con l'elemento rimosso nel corpo". Chiarimenti nel thread: l'endpoint non esiste e va aggiunto; con id inesistente risponde 404; va aggiunto anche sul web con un pulsante, gestendo correttamente gli errori che arrivano dal backend; il pulsante chiede conferma perché l'azione è distruttiva.

## Criteri di accettazione
- WHEN arriva DELETE /api/todos/{id} con un id esistente, l'API SHALL rimuovere l'elemento e rispondere 200 con l'elemento rimosso (`{ id, title, done }`) nel corpo.
- IF l'id non esiste THEN l'API SHALL rispondere 404.
- WHEN l'elemento è stato rimosso, GET /api/todos/{id} SHALL rispondere 404.
- WHEN l'utente preme "Elimina" su una riga, il web SHALL chiedere conferma prima di chiamare l'API.
- IF l'utente annulla la conferma THEN il web SHALL non chiamare l'API e lasciare la lista invariata.
- WHEN l'utente conferma, il web SHALL chiamare DELETE /api/todos/{id} e ricaricare la lista senza quell'elemento.
- IF l'API risponde 404 THEN il web SHALL mostrare "La cosa da fare non esiste più." e ricaricare la lista.
- IF l'API risponde con un altro errore THEN il web SHALL mostrare `errore <status>` senza modificare la lista.
- IF l'API non è raggiungibile THEN il web SHALL mostrare "API non raggiungibile."
