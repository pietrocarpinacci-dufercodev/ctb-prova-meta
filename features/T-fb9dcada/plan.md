# T-fb9dcada — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `Program.cs`, campo priorità sul modello `Todo` (vedi D2), test xUnit, riga in `README.md`.
- web: solo se D2 introduce la priorità nel contratto JSON (`web/src/todos.js` e test, come chiede il CLAUDE.md); se la priorità resta interna all'export, il web non cambia (vedi D5).

## Passi
1. (dopo D2) Aggiungere la priorità al record `Todo` con i valori `bassa|media|alta` del PDF; `NewTodo` accetta `priority` opzionale, default secondo D3; valore fuori elenco → 400 con `ValidationProblem`, come per il titolo vuoto.
2. Test xUnit (prima del codice): export vuoto = solo intestazione; due todo → righe `titolo,priorita` in ordine di id; titolo con virgola/apici quotato RFC 4180; `Content-Type: text/csv`; `GET /api/todos/1` continua a funzionare.
3. Endpoint in `Program.cs`: `app.MapGet("/api/todos/export", ...)` che costruisce il CSV con `StringBuilder` da `store.All()` e restituisce `Results.Text(csv, "text/csv")`. Nessuna libreria CSV: basta una funzione di escape di poche righe. La route non va in conflitto con `/api/todos/{id:int}` grazie al vincolo `int`.
4. Aggiornare la tabella endpoint di `api/README.md`.
5. Se D2/D5 lo richiedono: priorità in `web/src/todos.js` (`addTodo`) e nei test web.
6. Verifica: `dotnet test api` e, se toccato, `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori ammessi della priorità (`bassa`, `media`, `alta`).
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore virgola, nessun campo id/done.
- Allegato `/work/inputs/immagine-1.png`: un quadrato verde, nessuno schema leggibile (D1).
- `api/src/CtbProva.Api/Program.cs`: `Todo(Id, Title, Done)` senza priorità; route `/api/todos/{id:int}`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`.
- `CLAUDE.md` del meta-repo: un cambio di contratto API va riportato in `web/src/todos.js` e nei test di entrambi.
- `features/T-9dfe1d36/decisions.md`: execute delegato a `backend-dev`; nessun conflitto con questa feature.

## Rischio
Livello: giallo
- Costi: bassi, un endpoint e un campo sul modello.
- Complessità: bassa per l'export; media se la priorità entra nel contratto JSON e nel web.
- Sicurezza: CSV scaricabile da Excel → rischio di CSV/formula injection sui titoli che iniziano con `=`, `+`, `-`, `@` (D4); nessun dato reale, API senza autenticazione già oggi.
- Accoppiamento: se cambia `Todo` cambia il JSON di tutti gli endpoint e serve allineare il web (due PR).

## Domande aperte
- [ ] D1: L'immagine allegata è un quadrato verde senza contenuto leggibile: qual è lo schema da seguire? Va reinviata, o basta `dati.csv` come schema? — fonti: richiesta del thread ("lo schema nell'immagine") vs `/work/inputs/immagine-1.png`
- [ ] D2: Il modello `Todo` non ha la priorità, mentre il CSV la richiede: va aggiunta come campo dei todo (impostabile in POST e visibile nel JSON), o l'export deve usare un valore fisso? — fonti: `dati.csv` / `requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs` (`Todo(Id, Title, Done)`)
- [ ] D3: Se la priorità diventa un campo, qual è il default quando il POST non la indica (proposta: `media`) e che fare dei todo esistenti? — fonti: `requisiti.pdf` vs contratto attuale di `POST /api/todos`
- [ ] D4: Va neutralizzata la CSV/formula injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`), che però altera il testo esportato? — fonti: richiesta del thread vs formato di `dati.csv`
- [ ] D5: Il web deve offrire la scelta della priorità e/o un link "Esporta CSV", o la feature resta solo API? — fonti: richiesta del thread (solo endpoint) vs `CLAUDE.md` (contratto API riportato in `web/src/todos.js`)
- [ ] D6: L'export include solo `titolo,priorita` come `dati.csv` o anche `id` e `done`? Includere i todo già fatti? — fonti: `dati.csv` vs modello `Todo`
- [ ] D7: Dettagli di formato: fine riga (`\n` come `dati.csv` o `\r\n` RFC 4180), BOM UTF-8 per Excel, header `Content-Disposition: attachment; filename=...`? — fonti: `dati.csv` vs RFC 4180
