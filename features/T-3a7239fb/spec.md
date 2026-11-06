# T-3a7239fb — Elenco degli endpoint dell'API

## Richiesta
Nel thread Teams: «riesci a farmi l'elenco di tutti gli endpoint che sono attualmente presenti nel progetto API?». Richiesta di sola lettura: nessuna modifica al codice.

## Endpoint presenti (da `api/src/CtbProva.Api/Program.cs`)

| Metodo | Percorso | Risposte | Note |
| --- | --- | --- | --- |
| GET | `/api/health` | 200 `{ "status": "ok" }` | stato del servizio |
| GET | `/api/todos` | 200 con l'elenco ordinato per id | |
| GET | `/api/todos/{id:int}` | 200 con la cosa da fare, 404 se non esiste | |
| POST | `/api/todos` | 201 con `Location: /api/todos/{id}`, 400 (validation problem) se `title` è vuoto | corpo `{ "title": "..." }`, il titolo viene tagliato dagli spazi |
| POST | `/api/todos/{id:int}/done` | 200 con la cosa da fare aggiornata, 404 se non esiste | imposta `done = true` |

Altro: CORS aperto a qualunque origine, header e metodo; dati in memoria (`TodoStore`, singleton) persi al riavvio. Non ci sono controller, né altri `Map*` nel codice.

## Verifica
L'elenco coincide con la tabella in `api/README.md`; non ci sono discrepanze.

## Criteri di accettazione
- L'elenco è condiviso nel thread e corrisponde al codice attuale.
