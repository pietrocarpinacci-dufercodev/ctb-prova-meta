# T-ae9fa8a0 — Piano

## Submodule coinvolti
- api: `Todo` e `NewTodo` con `Priority`; enum/validazione dei valori `bassa|media|alta`; endpoint dedicato per cambiare la priorità; filtro `?priority=` su `GET /api/todos`; test in `TodoApiTests.cs`; tabella endpoint in `README.md`.
- web: `todos.js` (invio `priority` in `addTodo`, `sortTodos` per priorità), `app.js` (badge priorità colorato, menu in creazione), `index.html` e `style.css` (menu e tre colori), test in `web/tests/todos.test.js`, `README.md`.

## Passi
1. Api, test prima: casi di creazione con/senza priorità, valore non valido (400), modifica priorità (200/404/400), filtro (valido/non valido), elenco API ancora per id.
2. Api: aggiungere `Priority` a `Todo` (il `record` è posizionale: aggiornare `Add`, `MarkDone` usa già `with`); `NewTodo(string? Title, string? Priority)`; validazione che riusa lo stesso `ValidationProblem` del titolo; endpoint dedicato (D3); filtro in `GET /api/todos`; l'elenco API resta ordinato per id (D2).
3. Api: aggiornare `README.md` con i nuovi endpoint e parametri.
4. Web, test prima: `addTodo` invia `priority`; priorità sconosciuta/mancante gestita; `sortTodos` ordina non fatti per priorità alta→bassa poi id, fatti in fondo (D2).
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
- [x] D1: quale priorità di default quando in creazione non è indicata? — risposta: `media` (proposta accettata nel thread)
- [x] D2: quale ordine dell'elenco? — risposta: proposta accettata: dentro i non fatti, priorità decrescente (alta, media, bassa), poi id; i fatti in fondo per id. Lo applica il web in `sortTodos`; l'API resta ordinata per id (interpretazione del bot, vedi decisions.md)
- [x] D3: endpoint dedicato — risposta: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` (proposta accettata)
- [x] D4: valori e filtro — risposta: valori in italiano minuscolo `bassa|media|alta`, filtro `?priority=alta`; in input maiuscole/minuscole accettate, in output sempre minuscolo (interpretazione del bot)
- [x] D5: web su priorità esistente/filtro — risposta: no, il web ha solo il menu in creazione e il badge, come da richiesta; modifica e filtro restano solo API (interpretazione del bot)
