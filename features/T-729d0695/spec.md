# T-729d0695 — DELETE /api/todos/{id} con 200 e elemento rimosso

## Richiesta
Nel thread: "cambia DELETE /api/todos/{id} perché risponda 200 con l'elemento rimosso nel corpo".

## Criteri di accettazione
Bozza, da confermare dopo le risposte alle domande aperte (D1, D2).
- WHEN arriva DELETE /api/todos/{id} con un id esistente, l'API SHALL rimuovere l'elemento e rispondere 200 con l'elemento rimosso (`{ id, title, done }`) nel corpo.
- IF l'id non esiste THEN l'API SHALL rispondere 404 (da confermare, D2).
- WHEN l'elemento è stato rimosso, GET /api/todos/{id} SHALL rispondere 404.
