# T-729d0695 — Piano

## Submodule coinvolti
- api: aggiungere la rotta `DELETE /api/todos/{id:int}` in `Program.cs`, con un metodo `Remove(id)` in `TodoStore` (`ConcurrentDictionary.TryRemove`); 200 con il `Todo` rimosso nel corpo, 404 se l'id non esiste. Aggiornare `api/README.md` e `TodoApiTests.cs`.
- web: aggiungere `deleteTodo(id, fetchImpl)` in `web/src/todos.js`, un pulsante "Elimina" per ogni riga in `web/src/app.js` che chiede conferma prima di chiamare l'API (e stile in `style.css` se serve), la gestione degli errori del backend e i test in `web/tests/todos.test.js`.

## Passi
1. api: `TodoStore.Remove(int id)` che restituisce `Todo?`.
2. api: `app.MapDelete("/api/todos/{id:int}", ...)` → `Results.Ok(todo)` o `Results.NotFound()`.
3. api: test (200 con corpo `{ id, title, done }`, 404 su id mancante, GET successivo 404) e riga nella tabella degli endpoint del README.
4. web, `todos.js`: `deleteTodo(id, fetchImpl = fetch)` con `DELETE ${API_BASE}/api/todos/${id}`. Restituisce l'elemento rimosso se la risposta è ok. Gli errori diventano eccezioni con un messaggio leggibile:
   - 404: "La cosa da fare non esiste più." (l'elemento è già stato rimosso, per esempio da un'altra scheda);
   - altri stati: `errore <status>`, come le altre funzioni;
   - errore di rete (fetch che fallisce): "API non raggiungibile."
   Per distinguere il 404 l'eccezione porta lo stato (`error.status`).
5. web, `app.js`: pulsante "Elimina" su ogni riga (anche sulle cose già fatte). Al click si chiama `confirm(\`Eliminare "${todo.title}"?\`)`: se l'utente annulla non parte nessuna chiamata e la lista non cambia; se conferma si esegue `run(() => deleteTodo(todo.id))`. In caso di 404 la lista va comunque ricaricata (l'elemento non c'è più), oltre a mostrare il messaggio nell'elemento `#error`; negli altri errori la lista resta com'è e compare il messaggio. Le altre azioni (`Fatto`, `Aggiungi`) non cambiano comportamento.
6. web, test: `deleteTodo` invia `DELETE` all'URL giusto e restituisce l'elemento; 404 → messaggio "non esiste più" con `status` 404; altro stato → `errore 500`; fetch che lancia → "API non raggiungibile." La conferma sta in `app.js` (DOM), quindi non ha test automatici.
7. web: aggiornare `web/README.md` se serve.
8. Verifica: `dotnet test api` e `npm test --prefix web`. Non verificabile nel container: prova manuale del pulsante e della finestra di conferma nel browser (nessun test DOM nel progetto).

## Rischio
Livello: verde
- Costi: poche righe in due submodule.
- Complessità: bassa; store in memoria, nessuna dipendenza nuova.
- Sicurezza: l'endpoint è distruttivo e senza autenticazione, come gli altri; i dati sono di prova e in memoria. La conferma nel web riduce le eliminazioni accidentali ma non protegge l'API. Il titolo resta scritto con `textContent` (la conferma usa `confirm()`, che mostra testo semplice), quindi nessun rischio XSS aggiunto.
- Accoppiamento: nuovo endpoint riportato in `web/src/todos.js` e nei test di entrambi, come chiede CLAUDE.md. Il web dipende dal 404 per mostrare il messaggio specifico: i due submodule vanno rilasciati insieme.

## Domande aperte
- [x] D1: la richiesta dice "cambia DELETE", ma l'endpoint non esiste nel codice né nel README. Va aggiunto da zero? — risposta: sì, l'endpoint è da aggiungere.
- [x] D2: id inesistente: 404 o 200/204 idempotente? — risposta: 404.
- [x] D3: il web deve ottenere una funzione `deleteTodo` (e un pulsante)? — risposta: sì, anche sul web con pulsante e gestione corretta degli errori che arrivano dal backend.
- [x] D4: il pulsante deve chiedere conferma prima di eliminare? — risposta: sì, perché l'azione è distruttiva.
