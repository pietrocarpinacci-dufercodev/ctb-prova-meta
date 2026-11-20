# T-a355ab56 — Piano

## Submodule coinvolti
- api: campo `Priority` sul modello `Todo` (con validazione in POST), nuovo endpoint `GET /api/todos/export` in CSV, test xUnit, README.
- web: `addTodo` in `web/src/todos.js` invia la priorità (se D4 conferma), test in `web/tests/todos.test.js`; eventuale link "Esporta CSV" solo se richiesto (D6).

## Passi
1. api — modello: aggiungere `Priority` (string) a `Todo` e `NewTodo` in `api/src/CtbProva.Api/Program.cs`; valori ammessi `bassa`, `media`, `alta` (da `requisiti.pdf`); default per POST senza priorità secondo D3; 400 con `Results.ValidationProblem` sul campo `priority` se il valore non è ammesso, come già si fa per `title`. `TodoStore.Add` riceve la priorità.
2. api — export: `app.MapGet("/api/todos/export", ...)` che costruisce il CSV da `store.All()` con `StringBuilder` (nessuna dipendenza nuova): intestazione `titolo,priorita`, separatore `,`, righe `\n`, escaping RFC 4180 del titolo (campo tra doppi apici se contiene `,`, `"` o a capo; `"` raddoppiati). Risposta `Results.Text(csv, "text/csv; charset=utf-8")`. La rotta `/api/todos/{id:int}` ha il vincolo `int`, quindi `export` non va in conflitto.
3. api — test (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: content type; intestazione esatta; una riga per todo nel formato di `dati.csv` (`Comprare il latte,bassa`); escaping di un titolo con virgola e doppi apici; priorità non valida → 400; default della priorità; `GET /api/todos/{id}` invariato. Nota: la `WebApplicationFactory` è condivisa tra i test della classe, quindi i test sull'export verificano la presenza delle righe create e l'intestazione, non il conteggio totale (o usano una factory dedicata).
4. api — README: aggiungere la riga dell'endpoint `GET /api/todos/export` e il campo `priority` nel POST.
5. web — contratto (CLAUDE.md: un cambio al contratto va riportato in `web/src/todos.js` e nei test): `addTodo(title, priority, fetchImpl)` invia `priority` nel body; aggiornare `web/tests/todos.test.js` e la chiamata in `web/src/app.js`. Ambito dell'interfaccia secondo D4/D6.
6. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori della priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: colonne `titolo,priorita`, separatore virgola, nessun campo id/stato.
- Allegato `/work/inputs/immagine-1.png`: contiene solo un quadrato verde, nessuno schema leggibile (D1).
- `CLAUDE.md` del meta-repo: contratto API riportato in `web/src/todos.js` e nei test di entrambi; comandi di test.
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, store in memoria, validazione con `ValidationProblem`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`.
- `features/T-9dfe1d36/decisions.md`: nessun contrasto (riguarda l'agente `backend-dev`, da usare nell'execute).

## Rischio
Livello: giallo
- Costi: bassi, poche decine di righe nell'api e qualche riga nel web, nessuna dipendenza nuova.
- Complessità: bassa per l'export; media per l'aggiunta del campo `priority`, che cambia il modello e il contratto JSON.
- Sicurezza: export senza autenticazione come il resto dell'API (CORS aperto); il CSV può contenere titoli che iniziano con `=`, `+`, `-`, `@` (CSV injection in Excel, D5).
- Accoppiamento: il campo `priority` tocca il contratto tra api e web; lo schema dell'immagine non è leggibile, quindi il formato finale dipende dalle risposte.

## Domande aperte
- [ ] D1: L'allegato `immagine-1.png` mostra solo un quadrato verde, senza schema leggibile: potete ricaricare l'immagine con lo schema, oppure confermare che il formato è solo quello di `dati.csv` (`titolo,priorita`)? — fonti: richiesta nel thread ("lo schema nell'immagine") vs contenuto di `immagine-1.png`
- [ ] D2: Il CSV deve avere solo le colonne `titolo,priorita` di `dati.csv`, o anche `id` e lo stato `done` che il modello già ha? — fonti: `dati.csv` vs `Todo(Id, Title, Done)` in `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Il modello non ha la priorità: va aggiunto il campo `priority` ai todo (proposta: sì, con default `media` se il POST non la indica)? Quale default volete? — fonti: `requisiti.pdf` e `dati.csv` vs `Todo` in `Program.cs`
- [ ] D4: La pagina web deve permettere di scegliere la priorità alla creazione (select bassa/media/alta) o basta che `addTodo` invii il default? — fonti: CLAUDE.md (cambio di contratto riportato nel web) vs richiesta (solo endpoint di export)
- [ ] D5: Va applicata la protezione contro la CSV injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`), anche se altera il testo rispetto a `dati.csv`? — fonti: sicurezza dell'export vs formato di `dati.csv`
- [ ] D6: Serve un link/pulsante "Esporta CSV" nella pagina web, o l'endpoint basta da solo? — fonti: richiesta (solo endpoint) vs CLAUDE.md (contratto riportato in `web/src/todos.js`)
- [ ] D7: Servono BOM UTF-8 (per Excel) e l'intestazione `Content-Disposition: attachment; filename=...` con un nome file preciso? — fonti: `dati.csv` (UTF-8 senza BOM, nessun nome indicato) vs uso tipico dell'export
