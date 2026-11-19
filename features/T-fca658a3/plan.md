# T-fca658a3 — Piano

## Submodule coinvolti
- api: campo `Priority` su `Todo` e `NewTodo` con validazione, nuovo endpoint `GET /api/todos/export` in CSV, test e README.
- web: `priority` nell'oggetto inviato da `addTodo` in `web/src/todos.js` e nei test, solo se la risposta a D3 lo prevede.

## Passi
1. api — test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - l'export con todo creati restituisce 200, `text/csv` e il corpo atteso (`titolo,priorita` più una riga per ogni todo);
   - una priorità non valida su `POST /api/todos` dà 400;
   - senza priorità vale il default (D2);
   - un titolo con virgola o virgolette esce quotato secondo RFC 4180.
   Nota: la fixture `WebApplicationFactory` è condivisa tra i test e lo store è un singleton, quindi i test sull'export verificano le righe dei todo che creano (`Contains`), non l'intero corpo.
2. api — in `Program.cs`:
   - `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`;
   - in `POST /api/todos` validazione di `Priority` sui valori `bassa|media|alta` (array statico), con il default se manca;
   - `TodoStore.Add(title, priority)`.
3. api — endpoint `app.MapGet("/api/todos/export", ...)`:
   - costruisce il CSV con `StringBuilder` partendo da `store.All()`, con una funzione di escape locale (nessuna nuova dipendenza);
   - restituisce `Results.Text(csv, "text/csv; charset=utf-8")`.
   La route non va in conflitto con `/api/todos/{id:int}`, che ha il vincolo `int`.
4. api — aggiornare la tabella degli endpoint in `api/README.md` (nuova riga per l'export, campo `priority` sul POST).
5. web — solo se D3 lo richiede: il parametro opzionale `priority` in `addTodo` e il test relativo in `web/tests/todos.test.js`. Per il CLAUDE.md del meta-repo i cambi al contratto vanno riportati in `web/src/todos.js`: aggiungere un campo opzionale in risposta non rompe il web, quindi senza D3 il web non si tocca.
6. Verifica: `dotnet test api` e, se si tocca il web, `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori di priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore `,`, esempi `Comprare il latte,bassa` e `Preparare la demo,alta`.
- Allegato `/work/inputs/immagine-1.png`: un riquadro verde uniforme, senza schema leggibile (vedi D1).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, store in memoria, route esistenti.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`.
- `CLAUDE.md` del meta-repo: un cambio al contratto va riportato in `web/src/todos.js` e nei test.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda l'agente `backend-dev`).

## Rischio
Livello: verde
- Costi: piccolo intervento su un solo file di codice più i test; nessuna dipendenza nuova.
- Complessità: bassa. Si aggiunge un campo al modello, un endpoint e l'escape CSV.
- Sicurezza: l'API non ha autenticazione e l'export espone gli stessi dati di `GET /api/todos`. C'è il rischio di CSV injection se un titolo inizia con `=`, `+`, `-` o `@` e il file viene aperto in Excel (vedi D5).
- Accoppiamento: si estende il contratto del modello `Todo` (nuovo campo `priority` nel JSON), una modifica additiva che il web attuale ignora.

## Domande aperte
- [ ] D1: L'immagine allegata è un riquadro verde uniforme di pochi pixel e non mostra nessuno schema: potete allegare di nuovo lo schema o descriverlo (colonne, ordine, tipi)? Intanto il piano usa le colonne di `dati.csv`. — fonti: richiesta del thread ("lo schema nell'immagine") vs `immagine-1.png`
- [ ] D2: Oggi il modello `Todo` non ha la priorità, che invece è richiesta dal PDF e dal CSV. Va bene aggiungerla al modello e a `POST /api/todos`, come opzionale con default `media`? Oppure va resa obbligatoria o calcolata in altro modo? — fonti: `requisiti.pdf` e `dati.csv` vs `Program.cs` (`Todo(Id, Title, Done)`)
- [ ] D3: La pagina web deve permettere di scegliere la priorità o di scaricare l'export (link o bottone)? La richiesta parla solo dell'endpoint, ma il CLAUDE.md chiede di riportare i cambi di contratto in `web/src/todos.js`. — fonti: richiesta del thread vs `CLAUDE.md` del meta-repo
- [ ] D4: Il CSV deve contenere solo `titolo,priorita` come `dati.csv`, oppure anche `id` e lo stato `done`? E vanno esportati anche i todo già fatti? — fonti: `dati.csv` vs modello `Todo` (`Id`, `Done`)
- [ ] D5: Servono anche l'header `Content-Disposition: attachment; filename=...` (con quale nome file?) e la neutralizzazione della CSV injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`)? E vanno decisi la codifica (UTF-8 con o senza BOM, per gli accenti in Excel) e i fine riga (`\n` o `\r\n`)? — fonti: `dati.csv` (UTF-8 senza BOM, fine riga `\n`) vs RFC 4180 (`\r\n`)
