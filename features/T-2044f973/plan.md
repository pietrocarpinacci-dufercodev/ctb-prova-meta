# T-2044f973 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` (CSV), campo `Priority` su `Todo` e su `POST /api/todos`, test e README.
- web: `src/todos.js` (invio opzionale della priorità e funzione/link per l'export) e relativi test, come chiede il CLAUDE.md per i cambi di contratto — in ambito solo se confermato in D4.

## Passi
1. api — test prima (xUnit in `tests/CtbProva.Api.Tests/TodoApiTests.cs`):
   - export con lista vuota → solo `titolo,priorita`;
   - export dopo due POST → intestazione + righe nell'ordine per id, `Content-Type` `text/csv`;
   - titolo con virgola/doppi apici → quoting RFC 4180;
   - titolo che inizia con `=` → prefisso `'` (se confermato D5);
   - POST con `priority` non valida → 400; POST senza `priority` → default (D3).
   Nota: i test condividono la `WebApplicationFactory` e lo store in memoria; il test "lista vuota" va isolato con una factory propria o va verificato solo l'header.
2. api — `Program.cs`:
   - `record Todo(int Id, string Title, bool Done, string Priority)` e `record NewTodo(string? Title, string? Priority)`;
   - validazione di `priority` in `POST /api/todos` contro `bassa|media|alta`, default da D3;
   - `app.MapGet("/api/todos/export", ...)` che costruisce il CSV da `store.All()` e restituisce `Results.Text(csv, "text/csv; charset=utf-8")`; funzione statica di escape (quoting + eventuale prefisso anti-injection).
   - Separatore di riga `\n` come in `dati.csv` (o `\r\n` RFC 4180: vedi D6).
3. api — `README.md`: riga per `GET /api/todos/export` e campo `priority` nel POST.
4. web (solo se D4 = sì): adeguare `src/todos.js`/`app.js` al campo `priority` nella risposta JSON (aggiunto, non rompe la lettura esistente) ed eventuale link "Esporta CSV"; aggiornare i test Node.
5. Verifica: `dotnet test api` e, se il web è toccato, `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: "la priorita ha i valori bassa, media e alta".
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, righe `Comprare il latte,bassa` e `Preparare la demo,alta`.
- Allegato `/work/inputs/immagine-1.png`: immagine di pochi pixel a tinta unita verde, senza schema leggibile (D1).
- `api/src/CtbProva.Api/Program.cs`: `Todo(Id, Title, Done)` senza priorità; rotta `/api/todos/{id:int}`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`.
- `CLAUDE.md` del meta-repo (cambio di contratto API → aggiornare `web/src/todos.js` e test di entrambi).
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda solo l'agente `backend-dev`).

## Rischio
Livello: giallo
- Costi: bassi, poche decine di righe e test nell'api.
- Complessità: bassa per l'export; media perché serve un campo nuovo (`Priority`) nel modello e nel POST.
- Sicurezza: export di testo inserito dagli utenti in CSV → rischio CSV/formula injection se aperto in Excel (D5); endpoint senza autenticazione come il resto dell'API (CORS aperto).
- Accoppiamento: cambia il contratto JSON di `Todo` (campo in più) e `POST /api/todos`; tocca potenzialmente anche `web`.

## Domande aperte
- [ ] D1: L'immagine `immagine-1.png` è un quadrato verde di pochi pixel senza alcuno schema visibile: potete ricaricare lo schema? Fino ad allora il piano segue solo `dati.csv` e `requisiti.pdf` — fonti: richiesta nel thread vs allegato `immagine-1.png`
- [ ] D2: `Todo` oggi non ha la priorità che il CSV deve esportare: va aggiunto il campo `Priority` al modello e a `POST /api/todos` (proposta del piano), oppure l'export deve usare altro? — fonti: `dati.csv`/`requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Che priorità hanno i todo creati senza indicarla (proposta: `media`), e il campo resta opzionale nel POST? — fonti: `requisiti.pdf` vs `POST /api/todos` attuale
- [ ] D4: Il web è in ambito (selettore di priorità alla creazione e/o link "Esporta CSV"), o basta l'api? Il CLAUDE.md chiede di riportare i cambi di contratto in `web/src/todos.js` — fonti: richiesta nel thread (solo endpoint) vs `CLAUDE.md` meta-repo
- [ ] D5: Il CSV contiene solo `titolo,priorita` come `dati.csv` (niente `id` né `done`), e va applicata la protezione anti CSV-injection (prefisso `'` sui titoli che iniziano con `= + - @`), che però altera il titolo esportato? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D6: Fine riga `\n` come in `dati.csv` o `\r\n` come RFC 4180; serve il BOM UTF-8 per l'apertura corretta degli accenti in Excel, e un `Content-Disposition: attachment; filename=...`? — fonti: `dati.csv` vs RFC 4180
