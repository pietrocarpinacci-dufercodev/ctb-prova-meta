# T-5e3a8542 — Piano

## Submodule coinvolti
- api: campo `Priority` nel modello `Todo` e in `POST /api/todos` (opzionale, validato), nuovo endpoint `GET /api/todos/export` in CSV, test xUnit, tabella endpoint in `api/README.md`.
- web: solo se la risposta a D4 lo richiede (campo `priority` in `web/src/todos.js` e nei test, eventuale link di download).

## Passi
1. api, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - export vuoto: 200, `text/csv; charset=utf-8`, solo `titolo,priorita`;
   - export con due cose da fare create via POST (`bassa`, `alta`): righe nel formato di `dati.csv`, ordine per id;
   - titolo con virgola e doppi apici: quotato secondo RFC 4180;
   - `POST` con `priority` non valida: 400; senza `priority`: `media`.
   Nota: i test condividono la stessa `WebApplicationFactory` (store singleton), quindi il test dell'export vuoto va isolato con una factory propria o le asserzioni vanno fatte sulle righe create dal test.
2. api, `Program.cs`:
   - `record Todo(int Id, string Title, bool Done, string Priority)` e `record NewTodo(string? Title, string? Priority)`;
   - valori ammessi `bassa|media|alta` in un array statico; validazione nel `MapPost` accanto a quella del titolo; default `media`;
   - `TodoStore.Add(title, priority)`;
   - `app.MapGet("/api/todos/export", ...)` che costruisce il CSV con `StringBuilder` (nessuna dipendenza nuova) e restituisce `Results.Text(csv, "text/csv; charset=utf-8")` più `Content-Disposition`. Nessun conflitto di rotta con `/api/todos/{id:int}` grazie al vincolo `int`.
3. api, `api/README.md`: riga `GET /api/todos/export` nella tabella e campo `priority` nella riga del POST.
4. web (solo se D4 = sì): `addTodo(title, priority)` e test in `web/tests/todos.test.js`; nessuna modifica se il campo resta solo in uscita.
5. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori della priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore virgola, valori di priorità in minuscolo.
- Allegato `/work/inputs/immagine-1.png`: nessuno schema leggibile (immagine verde di pochi pixel), vedi D1.
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, store in memoria, validazione con `Results.ValidationProblem`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: stile dei test (xUnit + `WebApplicationFactory`).
- `api/README.md`: tabella degli endpoint.
- `web/src/todos.js`: contratto lato web (`title`, `done`, `id`).
- `CLAUDE.md` del meta-repo: un cambio di contratto dell'API va riportato in `web/src/todos.js` e nei test di entrambi.
- `features/T-9dfe1d36/decisions.md`: execute delegato all'agente `backend-dev`; nessun conflitto con questa feature.

## Rischio
Livello: giallo
- Costi: bassi, un endpoint e un campo, nessuna dipendenza nuova.
- Complessità: bassa, ma il campo `priority` cambia il modello e il contratto JSON di tutti gli endpoint.
- Sicurezza: il CSV contiene testo inserito dagli utenti; serve l'escaping RFC 4180 e va deciso se neutralizzare le formule (CSV injection) quando il file si apre in Excel.
- Accoppiamento: il cambio al contratto tocca anche il web secondo il `CLAUDE.md`; schema dell'immagine non leggibile, quindi il formato finale non è del tutto confermato.

## Domande aperte
- [ ] D1: `immagine-1.png` è un quadrato verde di pochi pixel senza schema leggibile: potete ricaricare lo schema o descriverlo? Finché manca, il piano segue solo `dati.csv` e `requisiti.pdf` — fonti: richiesta del thread ("lo schema nell'immagine") vs contenuto di `immagine-1.png`
- [ ] D2: il modello `Todo` non ha la priorità; confermate di aggiungere il campo `priority` al modello e a `POST /api/todos` (in alternativa l'export avrebbe una priorità fissa o vuota)? — fonti: `dati.csv` e `requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: quale priorità va assegnata quando il POST non la indica (proposta: `media`), e i valori nel JSON restano in italiano (`bassa|media|alta`) come nel PDF o vanno in inglese nel JSON e in italiano solo nel CSV? — fonti: `requisiti.pdf` vs regola "codice in inglese" del progetto
- [ ] D4: il campo `priority` va gestito anche nel web (scelta della priorità alla creazione, link "Esporta CSV") o il web resta invariato e ignora il campo nuovo? — fonti: `CLAUDE.md` (cambio di contratto da riportare nel web) vs richiesta del thread (solo endpoint API)
- [ ] D5: il CSV deve contenere solo `titolo,priorita` come `dati.csv` o anche `id` e `done`? Va confermato anche il nome del file scaricato (proposta: `todos.csv`) — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D6: neutralizziamo i titoli che iniziano con `=`, `+`, `-`, `@` (prefisso `'`) contro la CSV injection, accettando che il titolo esportato differisca da quello salvato? — fonti: richiesta "formato di dati.csv" vs sicurezza del file aperto in Excel
