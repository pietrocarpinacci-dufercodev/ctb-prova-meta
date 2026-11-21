# T-8bd2cd9d — Piano

## Submodule coinvolti
- api: campo `Priority` sul modello `Todo` con validazione in `POST /api/todos`, nuovo endpoint `GET /api/todos/export` in CSV, test e README.
- web: `addTodo` in `web/src/todos.js` accetta una priorità opzionale e i test si allineano al contratto (regola del `CLAUDE.md` sui cambi di contratto; ambito da confermare con D4).

## Passi
1. api, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - export con elenco vuoto → 200, `text/csv`, solo `titolo,priorita`;
   - export dopo due POST → intestazione e righe nell'ordine degli id, con le priorità;
   - titolo con virgola e doppi apici → campo quotato secondo RFC 4180;
   - POST senza priorità → `media`; POST con `priority: "urgente"` → 400;
   - `GET /api/todos/{id}` continua a funzionare (regressione sulla rotta).
   Dato che `WebApplicationFactory` è condivisa tra i test (`IClassFixture`), i test sull'export confrontano solo le righe create dal test stesso, oppure usano una factory dedicata per il caso "elenco vuoto".
2. api, modello in `api/src/CtbProva.Api/Program.cs`: `Todo(int Id, string Title, bool Done, string Priority)`, `NewTodo(string? Title, string? Priority)`; validazione dei valori `bassa|media|alta` accanto a quella del titolo; `TodoStore.Add(title, priority)`.
3. api, endpoint: `app.MapGet("/api/todos/export", ...)` che costruisce il CSV da `store.All()` con un piccolo helper di escaping RFC 4180 e risponde con `Results.File(bytes, "text/csv; charset=utf-8", "todos.csv")` (o `Results.Text` più l'intestazione `Content-Disposition`). La rotta `{id:int}` esistente ha già il vincolo `int`, quindi `export` non va in conflitto.
4. api, `api/README.md`: riga nella tabella endpoint per `GET /api/todos/export` e campo `priority` nel corpo del POST.
5. web, `web/src/todos.js`: `addTodo(title, priority, fetchImpl)` (o un oggetto opzioni) che invia `priority` solo se valorizzata; test in `web/` aggiornati. Nessun cambio alla UI se D4 non lo chiede.
6. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori ammessi della priorità (`bassa`, `media`, `alta`).
- Allegato `/work/inputs/dati.csv`: formato del CSV (intestazione `titolo,priorita`, separatore virgola, nessun quoting negli esempi).
- Allegato `/work/inputs/immagine-1.png`: esaminato, è un'immagine a tinta unita verde di pochi pixel senza schema leggibile (D1).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, rotte esistenti.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: stile dei test (xUnit con `WebApplicationFactory`).
- `api/README.md`: tabella degli endpoint.
- `web/src/todos.js`: client del contratto API.
- `CLAUDE.md` del meta-repo: un cambio di contratto va riportato in `web/src/todos.js` e nei test di entrambi.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto con questa feature.

## Rischio
Livello: giallo
- Costi: bassi, un endpoint e un campo in più in un'API di prova in memoria.
- Complessità: bassa per l'export; media per l'introduzione della priorità, che cambia il modello e il contratto del POST.
- Sicurezza: il CSV contiene testo inserito dagli utenti; senza neutralizzare i campi che iniziano con `=`, `+`, `-`, `@` un foglio di calcolo può interpretarli come formule (CSV injection, D6). Nessun dato sensibile, nessuna autenticazione nell'API.
- Accoppiamento: il campo `priority` cambia il JSON di risposta e del POST, quindi tocca anche il client web e i suoi test.

## Domande aperte
- [ ] D1: L'immagine `immagine-1.png` è un quadrato verde di pochi pixel e non mostra nessuno schema: potete ricaricare l'immagine corretta o descrivere lo schema (colonne, ordine, tipi)? Fino ad allora il piano segue solo `dati.csv` — fonti: richiesta del thread ("lo schema nell'immagine") vs `/work/inputs/immagine-1.png`
- [ ] D2: Il modello `Todo` non ha la priorità, ma il CSV la richiede: confermate di aggiungere un campo `priority` al modello e al `POST /api/todos` (scelta del piano), invece di esportare un valore fisso? — fonti: `dati.csv` e `requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Quale priorità assegnare alle cose da fare create senza priorità: `media` (proposta) o un'altra? — fonti: `requisiti.pdf` vs contratto attuale del `POST /api/todos`
- [ ] D4: Nel web basta allineare `addTodo` e i test al nuovo campo, o serve anche un selettore della priorità e/o un pulsante "Esporta CSV" nella pagina? — fonti: `CLAUDE.md` (cambio di contratto da riportare nel web) vs richiesta del thread (solo endpoint)
- [ ] D5: Il CSV deve contenere solo `titolo,priorita` come `dati.csv`, o anche `id` e lo stato `done`? E le cose già fatte vanno esportate o escluse? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D6: Dettagli del formato: fine riga CRLF (RFC 4180) o LF, BOM UTF-8 per Excel sì/no, e neutralizzazione delle formule (prefisso `'` per campi che iniziano con `=`, `+`, `-`, `@`) sì/no? — fonti: `dati.csv` (esempi senza questi casi) vs RFC 4180 e uso in fogli di calcolo
