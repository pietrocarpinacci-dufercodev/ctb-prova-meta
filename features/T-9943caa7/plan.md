# T-9943caa7 — Piano

## Submodule coinvolti
- api: `Todo` e `NewTodo` con campo `priority`; validazione sui tre valori; endpoint dedicato per cambiare la priorità; filtro `?priority=` su `GET /api/todos`; test in `TodoApiTests.cs`; tabella endpoint in `README.md`.
- web: `todos.js` (invio `priority` in `addTodo`, eventuale ordinamento in `sortTodos`); `index.html` (menu priorità nel form); `app.js` (badge priorità accanto al todo); `style.css` (un colore per valore); test in `web/tests/todos.test.js`; README se serve.

## Passi
1. API (TDD): aggiungere `Priority` al record `Todo` e a `NewTodo`; valore ammesso solo `bassa|media|alta`, altrimenti 400 con `ValidationProblem` su `priority`, come già fatto per `title`.
2. API: default alla creazione se `priority` assente (D1).
3. API: endpoint dedicato per modificare la priorità (forma in D3), 404 se id inesistente, 400 se valore non valido, con `TodoStore.SetPriority` sul modello di `MarkDone`.
4. API: filtro `GET /api/todos?priority=`; 400 se valore non valido; ordine dell'elenco secondo D2.
5. API: test per creazione con/senza priorità, valore non valido, modifica (ok/404/400), filtro (ok/400); aggiornare `api/README.md`.
6. Web: `addTodo(title, priority)` invia `priority`; menu `<select>` nel form con preselezione sul default; badge accanto al titolo in `render`; classi CSS `priority-bassa|media|alta` con colori distinti (testo visibile oltre al colore, per accessibilità).
7. Web: aggiornare `sortTodos` solo se D2 lo richiede; test in `todos.test.js`.
8. Eseguire `dotnet test api` e `npm test --prefix web`; scrivere le descrizioni PR in `/work/out`.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, contratto API da riportare in `web/src/todos.js` e nei test)
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/.../TodoApiTests.cs`
- `web/src/todos.js`, `app.js`, `index.html`, `style.css`
- Richiesta nel thread Teams; nessun allegato in `/work/inputs` usato; nessuna decisione di altre feature (`features/` contiene solo `_template`).

## Rischio
Livello: verde
- Costi: modifica piccola, due submodule, nessuna dipendenza nuova.
- Complessità: bassa; logica analoga a `MarkDone` e alla validazione del titolo.
- Sicurezza: input `priority` e filtro validati su whitelist; nessun segreto; dati in memoria.
- Accoppiamento: cambio del contratto API con effetto sul web; i due PR vanno rilasciati insieme (il web vecchio continua a funzionare perché `priority` in creazione è opzionale).

## Domande aperte
- [ ] D1: quale priorità di default quando non è indicata in creazione (`media`?) — fonti: richiesta nel thread (non decisa) vs spec
- [ ] D2: ordine dell'elenco: per id come oggi, per priorità (alta prima) dentro i non fatti, o altro? Vale per l'API, per il web o entrambi? — fonti: richiesta nel thread (non deciso) vs `sortTodos` in `web/src/todos.js` e `TodoStore.All` (oggi fatti in fondo / per id)
- [ ] D3: forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con `{ "priority": "alta" }` va bene, oppure `POST` come `/done`? — fonti: richiesta nel thread vs convenzione `POST .../done` in `api/README.md`
- [ ] D4: il web deve anche permettere di cambiare la priorità di un todo esistente, o solo sceglierla alla creazione? E il filtro per priorità serve anche nel web? — fonti: richiesta nel thread (menu solo in creazione, filtro solo nell'API)
- [ ] D5: valore di `priority` non valido o assente nel filtro: 400 per valori sconosciuti, va bene? Il confronto è case-insensitive? — fonti: richiesta nel thread vs spec
