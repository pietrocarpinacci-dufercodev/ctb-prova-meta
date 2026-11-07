# T-0e605945 — Piano

## Fonti
- Allegati: `requisiti-cancellazione 3.pdf` (R1–R6), `immagine-1.png` (mockup della conferma), `todos-esempio 3.csv` (dati di test).
- Repo: `CLAUDE.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`, `features/_template/*`.
- Spec: `features/T-0e605945/spec.md`.

## Submodule coinvolti
- api: `TodoStore.Remove(int id)` e `DELETE /api/todos/{id:int}` (204/404) in `Program.cs`; test; riga nuova nella tabella di `README.md`. CORS già consente ogni metodo.
- web: `deleteTodo(id, fetchImpl)` in `src/todos.js`; pulsante "Elimina" e conferma in `src/app.js` (e stile in `style.css` se serve); test in `tests/todos.test.js`.

## Passi
1. api: aggiungere `Remove(id)` a `TodoStore` (`_items.TryRemove`) e `app.MapDelete("/api/todos/{id:int}", ...)` → `Results.NoContent()` o `Results.NotFound()`.
2. api: copiare il CSV in `api/tests/CtbProva.Api.Tests/todos-esempio.csv` (copia nel build output). Helper di test che legge le righe e crea i todo via `POST` (e `/done` se `done=true`; gli id li assegna lo store, quindi i test usano gli id restituiti, non quelli del CSV).
3. api: test `DELETE` esistente → 204 e poi `GET` 404; todo completato (riga 2 del CSV) → 204; id inesistente → 404; doppia cancellazione → 204 poi 404; gli altri todo restano.
4. api: aggiornare `README.md` con la riga `DELETE /api/todos/{id}`.
5. web: `deleteTodo(id, fetchImpl = fetch)` come `markDone`: `DELETE`, errore se `!ok`, nessun body da leggere (204).
6. web: in `render`, un pulsante "Elimina" per ogni todo (anche completato); al click mostra la conferma con titolo (via `textContent`, mai `innerHTML`), Annulla/Elimina; conferma → `run(() => deleteTodo(todo.id))`, che già ricarica la lista (R5) e mostra gli errori.
7. web: copiare il CSV in `web/tests/todos-esempio.csv`; test di `deleteTodo` (URL, metodo `DELETE`, errore su 404) con `fetch` finto, e test su `sortTodos`/`summary` con i dati del CSV letti con `node:fs`.
8. Contratto: il cambio va riportato in `web/src/todos.js` e nei test di entrambi (`CLAUDE.md`). Eseguire `dotnet test api` e `npm test --prefix web`.
9. Commit locali `[T-0e605945] ...` su `claude/T-0e605945` in ogni submodule toccato; descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`.

## Rischio
Livello: verde
- Costi: piccoli, circa 4 file di codice e 2 di test; nessuna dipendenza nuova.
- Complessità: bassa; si segue il modello di `markDone` e `TodoStore`. Unica parte con un po' di UI è la conferma.
- Sicurezza: la cancellazione è irreversibile, ma i dati sono in memoria e di prova; la conferma è lato client. Il titolo nella conferma va inserito con `textContent` (no XSS). CORS già aperto a ogni origine, invariato.
- Accoppiamento: contratto API ↔ `web/src/todos.js`, da cambiare in entrambi i submodule; ordine di merge indifferente (il web finché l'API non ha DELETE riceve 404/405 e mostra l'errore).

## Domande aperte
- [ ] D1: Il mockup mostra una casella di spunta per segnare "fatto" su ogni riga, mentre la UI attuale ha il pulsante "Fatto" solo per i todo aperti. Va sostituito il pulsante con la casella (cambio fuori dalla richiesta) o la casella è solo grafica del mockup e si lascia "Fatto"? — fonti: `immagine-1.png` vs `web/src/app.js`
- [ ] D2: La conferma è un riquadro in pagina come nel mockup (si propone un `<dialog>`/riquadro non bloccante) oppure basta `window.confirm`? Il mockup ha "Annulla" ma il PDF mette "annulla" fuori ambito: si intende solo l'undo dopo la cancellazione? — fonti: `immagine-1.png` vs `requisiti-cancellazione 3.pdf`
- [ ] D3: R6 cita `todos-esempio.csv` ma l'allegato si chiama `todos-esempio 3.csv`, e il CSV ha 4 todo contro i 3 del mockup. Va bene copiarlo (rinominato `todos-esempio.csv`) come fixture in `api/tests` e `web/tests`, senza seed nell'app, e tenere tutte e 4 le righe? — fonti: `requisiti-cancellazione 3.pdf` vs `todos-esempio 3.csv`
