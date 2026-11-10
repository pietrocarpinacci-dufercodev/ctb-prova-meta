# T-ae9fa8a0 — Piano

## Submodule coinvolti
- api: `Todo` e `NewTodo` con `Priority`; enum/validazione dei valori `bassa|media|alta`; endpoint dedicato per cambiare la priorità; filtro `?priority=` su `GET /api/todos`; test in `TodoApiTests.cs`; tabella endpoint in `README.md`.
- web: `todos.js` (invio `priority` in `addTodo`, funzione per cambiare priorità se serve, eventuale ordinamento), `app.js` (badge priorità colorato, menu in creazione), `index.html` e `style.css` (menu e tre colori), test in `web/tests/todos.test.js`, `README.md`.

## Passi
1. Api, test prima: casi di creazione con/senza priorità, valore non valido (400), modifica priorità (200/404/400), filtro (valido/non valido), ordine dell'elenco.
2. Api: aggiungere `Priority` a `Todo` (il `record` è posizionale: aggiornare `Add`, `MarkDone` usa già `with`); `NewTodo(string? Title, string? Priority)`; validazione che riusa lo stesso `ValidationProblem` del titolo; endpoint dedicato (D3); filtro in `GET /api/todos`; ordine come da D2.
3. Api: aggiornare `README.md` con i nuovi endpoint e parametri.
4. Web, test prima: `addTodo` invia `priority`; priorità sconosciuta/mancante gestita; ordinamento se D2 lo richiede.
5. Web: menu `<select>` nel form, badge accanto al titolo con classe per valore, tre colori in `style.css` (colore non unico veicolo di significato: il testo della priorità resta visibile), `README.md`.
6. Eseguire `dotnet test api` e `npm test --prefix web`; l'API va rilasciata prima del web (il web invia un campo nuovo).
7. Nell'execute usare `backend-dev` per la parte API, come da CLAUDE.md; il web si fa senza agente dedicato (D4 di T-9dfe1d36).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, regola: un cambio al contratto va riportato in `web/src/todos.js` e nei test di entrambi).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`.
- `features/T-9dfe1d36/decisions.md` (agente `backend-dev`, nessun agente Frontend): nessuna contraddizione con questa feature.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: due submodule piccoli, in memoria, nessuna infrastruttura.
- Complessità: bassa; unico punto non banale è la validazione dei valori e l'ordine dell'elenco.
- Sicurezza: input utente nuovo (`priority` in body e query): va validato su lista chiusa; nel web usare `textContent`, come già fatto, mai `innerHTML`.
- Accoppiamento: contratto api↔web cambia in modo additivo (campo nuovo); il web deve tollerare todo senza `priority`.

## Domande aperte
- [ ] D1: quale priorità di default quando in creazione non è indicata (es. `media`)? — fonti: richiesta del thread ("non abbiamo deciso") vs spec (criteri con `<DEFAULT>`)
- [ ] D2: quale ordine dell'elenco? Oggi API per id e web prima i non fatti poi per id: si ordina per priorità (alta prima) dentro i non fatti, e l'ordine è fatto lato API, lato web o entrambi? — fonti: richiesta del thread vs `Program.cs` (`OrderBy(Id)`) e `web/src/todos.js` (`sortTodos`)
- [ ] D3: endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` va bene (coerente con `POST /{id}/done` sarebbe `POST`)? — fonti: richiesta del thread vs convenzione di `api/README.md`
- [ ] D4: i valori nel JSON sono in italiano minuscolo (`bassa`, `media`, `alta`) come scritti nella richiesta, e il filtro è `?priority=alta`? Maiuscole/minuscole accettate? — fonti: richiesta del thread vs convenzione dei nomi di campo in inglese (`title`, `done`)
- [ ] D5: il web deve anche permettere di cambiare la priorità di un todo esistente e/o filtrare l'elenco per priorità? La richiesta lo chiede solo per l'API (menu solo in creazione). — fonti: richiesta del thread (parte web) vs richiesta del thread (parte API)
