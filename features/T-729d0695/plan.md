# T-729d0695 — Piano

## Submodule coinvolti
- api: aggiungere (o cambiare, vedi D1) la rotta `DELETE /api/todos/{id:int}` in `Program.cs`, con un metodo `Remove(id)` in `TodoStore` (`ConcurrentDictionary.TryRemove`); 200 con il `Todo` rimosso, 404 se manca. Aggiornare `api/README.md` e `TodoApiTests.cs`.
- web: nessuna modifica obbligatoria; opzionale (D3) una `deleteTodo` in `web/src/todos.js` con test.

## Passi
1. api: `TodoStore.Remove(int id)` che restituisce `Todo?`.
2. api: `app.MapDelete("/api/todos/{id:int}", ...)` → `Results.Ok(todo)` o `Results.NotFound()`.
3. api: test (200 con corpo, 404 su id mancante, GET successivo 404) e riga nella tabella degli endpoint del README.
4. web: solo se D3 lo richiede, `deleteTodo` e test in `web/tests/todos.test.js`.
5. Verifica: `dotnet test api` e `npm test --prefix web`.

## Rischio
Livello: verde
- Costi: poche righe in un solo submodule.
- Complessità: bassa; store in memoria.
- Sicurezza: l'endpoint è distruttivo e senza autenticazione, come gli altri; i dati sono di prova e in memoria.
- Accoppiamento: è un nuovo endpoint o un cambio di contratto: CLAUDE.md chiede di riportarlo in `web/src/todos.js` e nei test di entrambi (vedi D3).

## Domande aperte
- [ ] D1: la richiesta dice "cambia DELETE", ma l'endpoint DELETE /api/todos/{id} non esiste nel codice né in `api/README.md` (ci sono solo GET, POST, POST done). Va aggiunto da zero? Se esiste in un altro branch, quale? — fonti: richiesta vs codice di api/Program.cs e README
- [ ] D2: id inesistente: 404 (come GET e done) o 200/204 idempotente? Il piano assume 404. — fonti: requisito ambiguo vs convenzione del codice esistente
- [ ] D3: il web deve ottenere una funzione `deleteTodo` (e un pulsante in `app.js`/`index.html`)? CLAUDE.md chiede di riportare i cambi di contratto in `web/src/todos.js` e nei test, ma la richiesta cita solo l'API. — fonti: richiesta vs CLAUDE.md del meta-repo
