# T-7b336c71 — Piano

## Submodule coinvolti
- api: campo `Priority` nel modello `Todo` e in `NewTodo` con validazione, endpoint `GET /api/todos/export` in CSV, test xUnit, README.
- web: solo se la risposta a D5 lo richiede (`priority` in `addTodo` e/o link di download in `web/src/todos.js` e `app.js`, con test `node --test`).

## Passi
1. api, test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`:
   - export con elenco vuoto → 200, `text/csv`, solo `titolo,priorita`;
   - export dopo due POST (`bassa`, `alta`) → righe come in `dati.csv`, in ordine di id;
   - titolo con virgola e doppi apici → campo quotato RFC 4180;
   - POST senza `priority` → `media`; POST con `priority` non valida → 400.
   - Nota: la fixture è condivisa (`IClassFixture`), quindi i test di export usano una `WebApplicationFactory` propria o verificano solo le righe che creano.
2. api, `Program.cs`:
   - `record Todo(int Id, string Title, bool Done, string Priority)` e `record NewTodo(string? Title, string? Priority)`;
   - valori ammessi `bassa`, `media`, `alta` in un array statico; validazione nel `MapPost` accanto a quella del titolo (`ValidationProblem` su `priority`);
   - `TodoStore.Add(title, priority)`;
   - `app.MapGet("/api/todos/export", ...)` che costruisce il CSV con `StringBuilder` da `store.All()` e restituisce `Results.Text(csv, "text/csv; charset=utf-8")`; escape RFC 4180 in un piccolo helper locale. Nessuna libreria CSV nuova.
3. api, `README.md`: riga `GET /api/todos/export` nella tabella e campo `priority` nel POST.
4. web (solo se D5 lo prevede): aggiornare `addTodo` per inviare `priority` e/o aggiungere `exportUrl()` in `web/src/todos.js`, con test; aggiornare `app.js`. Lo richiede la regola del `CLAUDE.md` del meta-repo sui cambi di contratto.
5. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori della priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore virgola, valori di priorità in minuscolo.
- Allegato `/work/inputs/immagine-1.png`: immagine 16×16 verde uniforme, nessuno schema leggibile (D1).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, rotta `/api/todos/{id:int}`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: test xUnit con `WebApplicationFactory`.
- `api/README.md`, `web/src/todos.js`.
- `CLAUDE.md` del meta-repo: un cambio di contratto dell'API va riportato in `web/src/todos.js` e nei test di entrambi.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguarda l'agente `backend-dev`).

## Rischio
Livello: giallo
- Costi: bassi, poche decine di righe in `Program.cs` e nei test, nessuna dipendenza nuova.
- Complessità: bassa per l'export; il campo `priority` cambia però il modello e il contratto JSON.
- Sicurezza: l'export espone tutti i todo senza autenticazione, come `GET /api/todos`; possibile CSV injection se un titolo inizia con `=`, `+`, `-`, `@` e il file viene aperto in Excel (D6).
- Accoppiamento: il contratto API cambia (nuovo campo `priority` nel JSON e nel POST), con possibile impatto su `web/` (D5).

## Domande aperte
- [ ] D1: L'immagine `immagine-1.png` è un quadrato verde 16×16 senza uno schema leggibile: potete allegare lo schema corretto o descriverlo (colonne, tipi, ordine)? — fonti: richiesta nel thread ("lo schema nell'immagine") vs allegato `immagine-1.png`
- [ ] D2: Il modello `Todo` non ha la priorità: confermate di aggiungere un campo `priority` al modello e al POST, o la priorità deve venire da un'altra parte? — fonti: `dati.csv` e `requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: Il CSV deve contenere solo `titolo,priorita` come `dati.csv`, o anche `id` e lo stato `done`? E vanno esportati anche i todo già fatti? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D4: Quale priorità ha un todo creato senza `priority` (proposta: `media`)? Un valore non valido va rifiutato con 400 (proposta) o normalizzato? I valori nel JSON restano in italiano (`bassa`/`media`/`alta`) o diventano un enum inglese (`low`/`medium`/`high`) tradotto solo nel CSV? — fonti: `requisiti.pdf` vs regola di lingua (codice in inglese)
- [ ] D5: La pagina web deve permettere di scegliere la priorità e/o offrire un link "Esporta CSV", o la feature resta solo sull'API? — fonti: `CLAUDE.md` del meta-repo (cambio di contratto da riportare in `web/src/todos.js`) vs richiesta (solo endpoint)
- [ ] D6: Requisiti di formato non indicati da `dati.csv`: BOM UTF-8 per Excel, fine riga `\n` o `\r\n` (RFC 4180), header `Content-Disposition: attachment; filename=...`, neutralizzazione della CSV injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`)? — fonti: `dati.csv` vs RFC 4180 e uso in Excel
