# T-3c27a032 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`/`NewTodo`, validazione, endpoint `PUT /api/todos/{id}/priority`, filtro `?priority=` su `GET /api/todos`, test, README.
- web: priorità accanto a ogni todo con colore per valore, menu alla creazione, `addTodo` con priorità, test, CSS.

## Passi
1. API (TDD): in `Program.cs` aggiungere la priorità come stringa minuscola `bassa|media|alta`, validata a mano contro l'elenco dei tre valori (la più semplice: valore ignoto o `null` esplicito → 400 con errore sul campo `priority`, mai 500).
2. API: `Todo(Id, Title, Done, Priority)`; `NewTodo(Title, Priority?)`; `TodoStore.Add` con priorità, default `media` se assente; nuovo `SetPriority(id, priority)` sul modello di `MarkDone`.
3. API: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }`: 200 con il todo aggiornato, 404 su id ignoto, 400 su valore non valido.
4. API: filtro `GET /api/todos?priority=...` (400 su valore non valido). Ordine dell'elenco invariato (per `Id`).
5. API: test in `TodoApiTests.cs` (default, creazione con priorità, valore non valido e `null`, modifica, 404, filtro, filtro non valido, ordine invariato); aggiornare `api/README.md`.
6. Web: `todos.js` — `addTodo(title, priority)` invia `priority`; costante con le tre priorità; `sortTodos` invariato; nessun filtro né modifica dopo la creazione nel web.
7. Web: `index.html`/`app.js`/`style.css` — `<select>` bassa/media/alta (preselezionato `media`), etichetta di priorità accanto a ogni todo con classe per colore (`prio-bassa|media|alta`), testo oltre al colore per l'accessibilità.
8. Web: test in `web/test` per `addTodo` con priorità e per `sortTodos` invariato; eseguire `dotnet test api` e `npm test --prefix web`.
9. Contratto riportato in entrambi i submodule (CLAUDE.md meta-repo); i due PR vanno rilasciati insieme.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, contratto API↔web, comandi).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`.
- Risposta nel thread: «Per tutte le domande aperte va bene la tua proposta».
- Nessun allegato in `/work/inputs` usato. Nessuna `decisions.md` di altre feature (esiste solo `features/_template`).

## Rischio
Livello: verde
- Costi: modifica piccola, due submodule, dati in memoria.
- Complessità: bassa.
- Sicurezza: validazione dell'input su creazione, modifica e filtro; nessun segreto.
- Accoppiamento: cambio di contratto API↔web (campo nuovo, retrocompatibile lato API grazie al default); i due PR vanno coordinati.

## Domande aperte
- [x] D1: Priorità di default — risposta: `media`
- [x] D2: Ordine dell'elenco — risposta: invariato (non fatti prima, poi per id), in API e web; nessun ordinamento per priorità
- [x] D3: Forma dell'endpoint dedicato — risposta: `PUT /api/todos/{id}/priority` con `{ "priority": "..." }`
- [x] D4: Filtro/modifica nel web — risposta: no, il web mostra la priorità e la sceglie solo alla creazione, come da richiesta; filtro e modifica restano solo nell'API
- [x] D5: Formato valori — risposta: minuscoli `bassa|media|alta`, 400 per valori ignoti incluso `null` esplicito
