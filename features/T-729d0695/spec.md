# T-729d0695 — DELETE /api/todos/{id} con 200 e elemento rimosso

## Richiesta
Nel thread: "cambia DELETE /api/todos/{id} perché risponda 200 con l'elemento rimosso nel corpo". Chiarimento nel thread: l'endpoint non esiste e va aggiunto; con id inesistente risponde 404.

## Criteri di accettazione
Resta aperta D3 (web), che non cambia i criteri dell'API.
- WHEN arriva DELETE /api/todos/{id} con un id esistente, l'API SHALL rimuovere l'elemento e rispondere 200 con l'elemento rimosso (`{ id, title, done }`) nel corpo.
- IF l'id non esiste THEN l'API SHALL rispondere 404.
- WHEN l'elemento è stato rimosso, GET /api/todos/{id} SHALL rispondere 404.
