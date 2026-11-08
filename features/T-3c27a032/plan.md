# T-3c27a032 — Piano

## Submodule coinvolti
- api: campo `priority` su `Todo`/`NewTodo`, validazione, endpoint dedicato di modifica, filtro `?priority=` su `GET /api/todos`, test, README.
- web: priorità accanto a ogni todo con colore per valore, menu alla creazione, `addTodo` con priorità, ordinamento secondo decisione, test, CSS.

## Passi
1. API (TDD): in `Program.cs` aggiungere la priorità come enum `Priority { Bassa, Media, Alta }` serializzata come stringa minuscola (`bassa|media|alta`) tramite `JsonStringEnumConverter` con naming lowercase, oppure stringa validata a mano (la più semplice che rifiuta valori ignoti con 400, non 500/deserializzazione generica).
2. API: `Todo(Id, Title, Done, Priority)`; `NewTodo(Title, Priority?)`; `TodoStore.Add` con priorità (default da D1); nuovo `SetPriority(id, priority)` sul modello di `MarkDone`.
3. API: endpoint dedicato di modifica (verbo/percorso da D3, proposta: `PUT /api/todos/{id}/priority` con `{ "priority": "alta" }`), 404 su id ignoto, 400 su valore non valido.
4. API: filtro `GET /api/todos?priority=...` (400 su valore non valido); ordine dell'elenco secondo D2 (oggi: per `Id`).
5. API: test in `TodoApiTests.cs` (default, creazione con priorità, valore non valido, modifica, 404, filtro, filtro non valido, ordine); aggiornare `api/README.md`.
6. Web: `todos.js` — `addTodo(title, priority)` invia `priority`; eventuale `fetchTodos(priority)` solo se D4 prevede il filtro nel web; `sortTodos` secondo D2; costante con le tre priorità.
7. Web: `index.html`/`app.js`/`style.css` — `<select>` con bassa/media/alta (preselezionata = default D1), etichetta di priorità accanto a ogni todo con classe per colore (es. `prio-bassa|media|alta`), colori con contrasto sufficiente e testo (non solo colore) per accessibilità.
8. Web: test in `web/test` per `addTodo`, `sortTodos` e normalizzazione; `dotnet test api` e `npm test --prefix web`.
9. Contratto riportato in entrambi i submodule (CLAUDE.md meta-repo); i due PR vanno rilasciati insieme: web nuovo con API vecchia mostrerebbe priorità assente, API nuova con web vecchio funziona (default).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, contratto API↔web, comandi).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`.
- Nessun allegato in `/work/inputs` usato. Nessuna `decisions.md` di altre feature trovata (esiste solo `features/_template`).

## Rischio
Livello: verde
- Costi: modifica piccola, due submodule, dati in memoria.
- Complessità: bassa; unica scelta delicata è la serializzazione dell'enum.
- Sicurezza: validazione dell'input su creazione, modifica e filtro; nessun segreto.
- Accoppiamento: cambio di contratto API↔web (campo nuovo, retrocompatibile lato API grazie al default); i due PR vanno coordinati.

## Domande aperte
- [ ] D1: Qual è la priorità di default quando non viene indicata in creazione (proposta: `media`)? — fonti: richiesta nel thread (non decisa) vs spec
- [ ] D2: Qual è l'ordine dell'elenco: invariato (non fatti prima, poi id) o per priorità (alta→bassa) dentro i non fatti? Vale per API, web o entrambi? — fonti: richiesta nel thread (non deciso) vs `web/src/todos.js` `sortTodos` / `Program.cs` `All()`
- [ ] D3: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (proposta) oppure `POST` come `/done`? — fonti: richiesta nel thread vs convenzione di `/done` in `api/README.md`
- [ ] D4: Il web deve anche filtrare per priorità e/o permettere di cambiarla dopo la creazione? La richiesta cita nel web solo visualizzazione e menu alla creazione — fonti: richiesta nel thread (parte API vs parte web)
- [ ] D5: Valori in minuscolo `bassa|media|alta` nel JSON (proposta) e 400 per valori ignoti, incluso `null` esplicito? — fonti: richiesta nel thread vs comportamento attuale di validazione del titolo
