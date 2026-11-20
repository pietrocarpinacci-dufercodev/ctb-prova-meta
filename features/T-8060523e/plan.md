# T-8060523e — Piano

## Submodule coinvolti
- api: campo `Priority` sul record `Todo` e su `NewTodo` con validazione, nuovo endpoint `GET /api/todos/export` (CSV), test xUnit, tabella endpoint nel README.
- web: solo `web/src/todos.js` e test se si conferma l'invio della priorità o un link di download (D6); altrimenti nessuna modifica.

## Passi
1. api — test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - export con zero todo → solo `titolo,priorita` (usare una factory dedicata o confrontare solo intestazione e righe create nel test, perché lo store è condiviso tra i test della classe);
   - export dopo due POST (`bassa` e senza priorità) → righe con `bassa` e `media`, ordine per id, `Content-Type` `text/csv` e `Content-Disposition` con `todos.csv`;
   - titolo con virgola e doppi apici → campo quotato RFC 4180;
   - POST con priorità non valida → 400;
   - `GET /api/todos/9999` resta 404 (test già esistente).
2. api — `Program.cs`:
   - `Todo(int Id, string Title, bool Done, string Priority)` e `NewTodo(string? Title, string? Priority)`; valori ammessi `bassa|media|alta`, default `media`; validazione nel `MapPost` accanto a quella del titolo (stesso stile `ValidationProblem`).
   - `TodoStore.Add(title, priority)`.
   - `app.MapGet("/api/todos/export", ...)` che costruisce il CSV con `StringBuilder` (nessuna dipendenza nuova) e restituisce `Results.Text(csv, "text/csv; charset=utf-8")` più l'header `Content-Disposition`. Funzione locale per l'escaping RFC 4180. La rotta letterale `export` non collide con `{id:int}`.
3. api — `README.md`: riga `GET /api/todos/export` nella tabella e campo `priority` nel POST.
4. web — solo se D6 lo chiede: `priority` opzionale in `addTodo` e/o funzione `exportUrl()` in `todos.js`, con test in `web/tests/todos.test.js`. Il campo `priority` in più nelle risposte JSON non rompe il web attuale.
5. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori della priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: formato CSV (intestazione `titolo,priorita`, separatore virgola, nessuna colonna id/done).
- Allegato `/work/inputs/immagine-1.png`: letto, ma contiene solo un quadrato verde senza schema (D1).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, `TodoStore` in memoria.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: stile dei test (WebApplicationFactory, xUnit).
- `api/README.md`, `web/src/todos.js`, `CLAUDE.md` del meta-repo (cambi di contratto riportati anche nel web e nei test).
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda l'agente `backend-dev`, da usare nell'execute).

## Rischio
Livello: giallo
- Costi: bassi, poche decine di righe in `Program.cs` e nei test, nessuna dipendenza nuova.
- Complessità: bassa per l'export; il campo priorità cambia il modello e il contratto del POST.
- Sicurezza: export senza autenticazione come il resto dell'API; CORS aperto già oggi. Possibile CSV injection (titoli che iniziano con `=`, `+`, `-`, `@`) se il file viene aperto in Excel (D5).
- Accoppiamento: il cambio del modello `Todo` tocca il contratto JSON usato dal web; lo schema dell'immagine non è leggibile, quindi i requisiti non sono completi.

## Domande aperte
- [ ] D1: L'immagine allegata (`immagine-1.png`) è un quadrato verde uniforme senza alcuno schema leggibile: potete ricaricare lo schema corretto o descriverlo nel thread? Il piano assume che lo schema coincida con `dati.csv` — fonti: richiesta del thread ("lo schema nell'immagine") vs contenuto di `immagine-1.png`
- [ ] D2: Il modello `Todo` non ha la priorità, richiesta da `requisiti.pdf` e `dati.csv`: confermate di aggiungere `Priority` al modello e un campo `priority` opzionale (validato) a `POST /api/todos`? In alternativa l'export scrive una priorità fissa — fonti: `requisiti.pdf` e `dati.csv` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Le colonne dell'export sono solo `titolo,priorita` come in `dati.csv` (senza `id` e `done`), con intestazione in italiano e valori di priorità in italiano anche nel JSON? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)` e regola lingua (codice in inglese)
- [ ] D4: Che priorità hanno i todo creati senza indicarla (proposta: `media`)? — fonti: `requisiti.pdf` (solo i valori ammessi) vs `POST /api/todos` attuale
- [ ] D5: Formato tecnico del CSV: separatore virgola come `dati.csv` (non punto e virgola per Excel italiano), UTF-8 senza BOM, fine riga `\n` o `\r\n`, quoting RFC 4180, nome file `todos.csv`, protezione da CSV injection sì/no? — fonti: `dati.csv` (solo esempio, senza casi limite) vs requisiti non specificati
- [ ] D6: Il web deve cambiare (scelta della priorità nel form, link "Esporta CSV")? Il CLAUDE.md chiede di riportare i cambi di contratto in `web/src/todos.js`, ma la richiesta parla solo dell'endpoint — fonti: `CLAUDE.md` del meta-repo vs richiesta del thread
- [ ] D7: `dati.csv` è solo un esempio di output o va usato anche come dato iniziale (seed) dello store? — fonti: richiesta del thread ("il formato del file dati.csv") vs contenuto con righe di dati reali
