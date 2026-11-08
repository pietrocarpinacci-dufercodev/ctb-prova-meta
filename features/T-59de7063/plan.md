# T-59de7063 — Piano

## Submodule coinvolti
- api (ctb-prova-api): campo `priority` (`bassa`, `media`, `alta`) in `Todo`; `NewTodo` accetta `priority` opzionale; nuovo endpoint per cambiare la priorità; filtro `?priority=` su `GET /api/todos`; test in `TodoApiTests.cs`; tabella endpoint in `api/README.md`.
- web (ctb-prova-web): `web/src/todos.js` (`addTodo(title, priority)`, nuova `setPriority`, ordinamento), `web/src/app.js` (priorità accanto a ogni todo con classe di colore), `web/src/index.html` (menu priorità nel form), `web/src/style.css` (tre colori), test in `web/tests/todos.test.js`.
- meta-repo: solo `features/T-59de7063/*` (CLAUDE.md già richiede di riportare i cambi di contratto in `web/src/todos.js` e nei test di entrambi).

## Passi
1. API: in `Program.cs` aggiungere il campo `Priority` a `Todo` (valori `bassa|media|alta`, serializzati come stringhe minuscole; enum con `JsonStringEnumConverter` o stringa validata). Valore di default: vedi D1.
2. API: `POST /api/todos` accetta `priority` opzionale; se assente usa il default (D1); se non valida risponde 400 con `ValidationProblem` su `priority`.
3. API: nuovo endpoint dedicato per modificare la priorità (proposta: `PUT /api/todos/{id}/priority` con `{ "priority": "alta" }`; 404 se il todo non esiste, 400 se il valore non è valido). Forma da confermare in D3.
4. API: `GET /api/todos?priority=alta` filtra; senza parametro restituisce tutto; valore non valido → 400 (D4). Ordine dell'elenco: vedi D2.
5. API: test (TDD) per default, creazione con priorità, valore non valido, cambio priorità (ok/404/400), filtro (ok/non valido), ordine. `dotnet test api`.
6. API: aggiornare `api/README.md` (tabella endpoint, campo `priority`).
7. Web: `todos.js` — `addTodo` invia `priority`; `setPriority(id, priority)`; eventuale funzione/ordinamento coerente con D2; test in `todos.test.js`. Il filtro per priorità non è richiesto nel web (D5).
8. Web: `index.html`/`app.js`/`style.css` — `<select>` bassa/media/alta nel form di creazione (preselezionato sul default D1); etichetta di priorità accanto a ogni todo con colore distinto per livello (non solo colore: il testo resta visibile). `npm test --prefix web`.
9. Nei due submodule committare su `claude/T-59de7063`; descrizioni PR in `/work/out` (execute).

## Fonti
- `CLAUDE.md` del meta-repo (struttura, contratto API/web, comandi di test).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`.
- `features/_template/*`. Nessuna altra feature con `decisions.md`; nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: nessuno, dati in memoria, nessuna dipendenza nuova.
- Complessità: bassa; poche modifiche in due submodule piccoli.
- Sicurezza: bassa; la priorità è un valore ristretto a tre stringhe, validato lato API; nel web si usa `textContent` (niente HTML iniettato).
- Accoppiamento: medio-basso; il contratto cambia in modo retro-compatibile (campo nuovo, parametro opzionale), ma API e web vanno rilasciati insieme per mostrare la priorità.

## Domande aperte
- [ ] D1: Quale priorità di default per i todo creati senza `priority` (e preselezionata nel menu web): `media`, `bassa` o altro? — fonti: richiesta del thread (non deciso) vs spec
- [ ] D2: Quale ordine per l'elenco: solo per id come oggi, oppure per priorità (alta prima) dentro i gruppi fatti/da fare? L'ordine vale in API, nel web o in entrambi? — fonti: richiesta del thread (non deciso) vs `web/src/todos.js` (`sortTodos` ordina per fatto, poi id)
- [ ] D3: Forma dell'endpoint dedicato: `PUT /api/todos/{id}/priority` con body `{ "priority": "alta" }` va bene, o si preferisce `POST` (coerente con `/done`) o `PATCH`? — fonti: `api/README.md` (usa POST per `/done`) vs richiesta ("endpoint dedicato")
- [ ] D4: Valore di `priority` non valido nel filtro o nel body: 400 con errore di validazione (proposta), e il confronto è case-insensitive? — fonti: richiesta del thread (non specificato) vs convenzione 400 di `POST /api/todos`
- [ ] D5: Il filtro per priorità serve anche nella pagina web, o solo nell'API come scritto nella richiesta? — fonti: richiesta del thread (filtro citato solo per l'API)
