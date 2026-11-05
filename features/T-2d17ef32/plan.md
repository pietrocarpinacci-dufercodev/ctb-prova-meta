# T-2d17ef32 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/count` con test (già implementato, commit locale `f3145bf`).
- web: contatore delle cose da fare nella home che usa `GET /api/todos/count` (richiesta attuale). Il contratto dell'API non cambia rispetto al piano precedente.

## Passi
### api (invariato)
1. `Program.cs`: `GET /api/todos/count` → `{ "count": N }` e `TodoStore.Count()`.
2. Test `Count_reflects_created_todos` (conteggio relativo, factory condivisa).
3. Riga nella tabella endpoint di `api/README.md`.
4. `dotnet test api`, commit `[T-2d17ef32] ...` sul branch `claude/T-2d17ef32`.

### web (nuovo)
5. `web/src/todos.js`: `fetchCount(fetchImpl)` che chiama `${API_BASE}/api/todos/count`, solleva `errore <status>` se la risposta non è ok e restituisce `count`; `countLabel(n)` per il testo ("1 cosa da fare in totale" / "N cose da fare in totale").
6. `web/src/index.html`: aggiungere `<p id="counter" class="counter" aria-live="polite">` sopra l'elenco.
7. `web/src/app.js`: in `run()`, dopo `render(await fetchTodos())`, chiamare `fetchCount()` e scrivere `countLabel(...)` in `#counter`. Così il contatore si aggiorna al caricamento, all'aggiunta e al "Fatto"; gli errori passano da `showError`.
8. `web/src/style.css`: regola `.counter` (grigio `#6b7383`, stessi colori del progetto, nessuna nuova dipendenza).
9. `web/tests/todos.test.js`: test per `fetchCount` (URL e valore), per l'errore HTTP e per `countLabel` (0, 1, N).
10. `web/README.md`: nota sul contatore.
11. `npm test --prefix web` (nel container `npm` può mancare: equivale a `node --test`), poi commit `[T-2d17ef32] ...` sul branch `claude/T-2d17ef32` del submodule `web`.

## Stato attuale
Nel working tree locale esistono già i commit `14f3eb6` (pagina dedicata `count.html`/`count.js` con `fetchCount`) e `054e488` (contatore nella home con `countLabel`) sul branch del submodule `web`. I passi 5–11 li coprono: in esecuzione vanno verificati, non rifatti. La pagina `count.html` era stata chiesta in una richiesta precedente e resta.

## Domande aperte
- Formato della risposta: `{ "count": N }` (coerente con `/api/health`).
- Il numero è il totale, comprese le cose già fatte, perché l'endpoint non le distingue. Se serve "ancora da fare", va aggiunto un parametro o un campo all'API (cambio di contratto: aggiornare anche `todos.js` e i test).
- Se il contatore deve restare in home insieme alla pagina dedicata `count.html`, o se una delle due va tolta.
