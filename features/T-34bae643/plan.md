# T-34bae643 — Piano

## Submodule coinvolti
- api: campo priorità sul modello `Todo`, endpoint `GET /api/todos/export` in CSV, test e README (tabella endpoint e sezione `## Glossario`).
- web: solo se la risposta a D2/D5 lo richiede (campo `priority` nel contratto JSON e/o link di download).

## Passi
1. api, test prima (xUnit in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`): export con elenco vuoto (solo intestazione), export con due todo (intestazione `titolo,priorita` + righe), titolo con virgola/doppio apice quotato RFC 4180, `Content-Type` `text/csv; charset=utf-8`.
2. api, modello (`api/src/CtbProva.Api/Program.cs`): aggiungere la priorità a `Todo` con i valori `bassa`, `media`, `alta`; comportamento di default e in input secondo D2. Se arriva in input un valore fuori elenco: 400 come per il titolo vuoto.
3. api, endpoint: `app.MapGet("/api/todos/export", ...)` che costruisce il CSV da `store.All()` con `StringBuilder` e restituisce `Results.Text(csv, "text/csv; charset=utf-8")`. Nessuna libreria CSV: due colonne e una funzione di escape di poche righe. La rotta esistente `/api/todos/{id:int}` ha il vincolo `int`, quindi non entra in conflitto.
4. api: aggiornare la tabella endpoint in `api/README.md`.
5. web (condizionato a D2/D5): se `priority` entra nel JSON, aggiornare `web/src/todos.js` e `web/tests/todos.test.js` come chiede il CLAUDE.md del meta-repo.
6. api: aggiungere in fondo a `api/README.md` (salvo diversa risposta a D8) una sezione `## Glossario` con le 40 voci della sezione Glossario di `spec.md`, stesso ordine e stesso testo. Se le risposte a D2–D7 cambiano il comportamento, aggiornare le voci collegate (7, 13, 24, 26, 28, 29, 30) prima di copiarle.
7. Verifica: `dotnet test api` e, se toccato, `npm test --prefix web`; controllo che il Glossario del README abbia 40 voci identiche a quelle di `spec.md`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori della priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: formato `titolo,priorita`, separatore virgola, nessun quoting negli esempi.
- Allegato `/work/inputs/immagine-1.png`: immagine quasi vuota (riquadro verde chiaro), nessuno schema leggibile.
- `api/src/CtbProva.Api/Properties/launchSettings.json` e `CtbProva.Api.csproj`: porta 5000 e `net10.0`, citati nel Glossario.
- `web/README.md`, `web/tests/todos.test.js`: fonti delle voci del Glossario sul web.
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, rotte esistenti.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js` e `CLAUDE.md` del meta-repo: un cambio al contratto va riportato nel web.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda solo l'agente `backend-dev`).

## Rischio
Livello: verde
- Costi: nessuna dipendenza nuova, poche righe nell'api.
- Complessità: bassa; l'unico punto delicato è l'escape CSV.
- Sicurezza: export pubblico come `GET /api/todos` (nessuna autenticazione nel progetto); valutare la CSV injection (titoli che iniziano con `=`, `+`, `-`, `@`) se il file viene aperto in Excel, vedi D6.
- Accoppiamento: aggiungere la priorità a `Todo` cambia il contratto JSON usato dal web.

## Domande aperte
- [ ] D1: L'immagine `immagine-1.png` non contiene uno schema leggibile (è un riquadro quasi vuoto): potete ricaricare lo schema o confermare che il formato è solo quello di `dati.csv`? — fonti: richiesta nel thread vs `/work/inputs/immagine-1.png`
- [ ] D2: Il modello `Todo` non ha una priorità: va aggiunta al modello (con default, es. `media`, quando non indicata) e accettata in `POST /api/todos`, oppure l'export deve scrivere un valore fisso? — fonti: `dati.csv`/`requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Il CSV deve contenere solo `titolo,priorita` come `dati.csv`, o anche `id` e lo stato `done`? E vanno esportati anche i todo già fatti? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D4: Fine riga e codifica: CRLF (RFC 4180) o LF? Con BOM UTF-8 per l'apertura corretta delle lettere accentate in Excel o senza? — fonti: `dati.csv` vs RFC 4180
- [ ] D5: Il web deve mostrare la priorità e/o offrire un link "Esporta CSV", o la feature riguarda solo l'api? — fonti: richiesta nel thread vs `CLAUDE.md` del meta-repo (contratto riportato in `web/src/todos.js`)
- [ ] D6: Serve neutralizzare la CSV injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`) o il CSV resta fedele al titolo? — fonti: `dati.csv` vs rischio sicurezza
- [ ] D7: Serve l'header `Content-Disposition: attachment; filename=...` per il download, e con quale nome file (es. `todos.csv`)? — fonti: richiesta nel thread vs nessuna indicazione negli allegati
- [ ] D8: In quale README va il Glossario: `api/README.md` (scelta del piano, è il submodule della feature), `web/README.md` o il `README.md` del meta-repo? — fonti: richiesta nel thread («nel README») vs tre README nel repository
- [ ] D9: Il Glossario riguarda tutto il progetto e non l'export CSV: va tenuto in questa feature o spostato in una feature separata di documentazione? — fonti: richiesta nel thread vs titolo e richiesta iniziale di T-34bae643
