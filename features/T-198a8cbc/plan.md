# T-198a8cbc — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `src/CtbProva.Api/Program.cs`, campo priorità nel modello (se confermato da D1), test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga nella tabella endpoint di `README.md`.
- web: solo se D1 e D4 lo confermano — campo priorità in `addTodo` di `src/todos.js` e relativi test (regola del CLAUDE.md: un cambio al contratto dell'API va riportato nel web).

## Passi
1. (api, se D1 = sì) Aggiungere al record `Todo` un campo `Priority` (stringa tra `bassa`, `media`, `alta`) e a `NewTodo` un `Priority?` opzionale con il default deciso in D2; validare in `POST /api/todos` con `ValidationProblem` (400) come già fatto per il titolo.
2. (api) Test prima del codice (TDD), in `TodoApiTests.cs`:
   - export con elenco vuoto → 200, `text/csv`, solo `titolo,priorita`;
   - export dopo due POST → intestazione più due righe in ordine di id;
   - titolo con virgola e doppi apici → campo tra apici con apici raddoppiati;
   - priorità non valida in POST → 400 (se D1 = sì);
   - `GET /api/todos/{id}` invariato.
   Nota: i test condividono la stessa `WebApplicationFactory` e quindi lo stesso `TodoStore`: i test sull'export devono usare una factory propria o confrontare solo le righe che creano.
3. (api) Mappare `app.MapGet("/api/todos/export", ...)` in `Program.cs`: costruire il testo con `StringBuilder` (intestazione `titolo,priorita`, una riga per `store.All()`, escape RFC 4180 in una piccola funzione locale) e restituire `Results.Text(csv, "text/csv; charset=utf-8")`. Nessuna dipendenza nuova. La rotta non collide con `/api/todos/{id:int}` grazie al vincolo `int`.
4. (api) Aggiornare la tabella endpoint in `api/README.md`.
5. (web, solo se D4 = sì) Aggiungere la priorità a `addTodo` in `web/src/todos.js` e i test in `web/tests/todos.test.js`; un eventuale link/pulsante "Esporta CSV" solo se richiesto in D4.
6. Verifica: `dotnet test api` e, se il web è toccato, `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori ammessi della priorità (`bassa`, `media`, `alta`).
- Allegato `/work/inputs/dati.csv`: formato del CSV (intestazione `titolo,priorita`, separatore virgola, nessun campo tra apici).
- Allegato `/work/inputs/immagine-1.png`: immagine di pochi pixel, verde e uniforme; nessuno schema leggibile (vedi D3).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)`, `TodoStore`, rotte esistenti.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: stile dei test (xUnit + `WebApplicationFactory`).
- `api/README.md`: tabella degli endpoint.
- `web/src/todos.js`: contratto lato web.
- `CLAUDE.md` del meta-repo: regola sul contratto API ↔ web e comandi di test.
- `features/T-9dfe1d36/decisions.md`: nessun conflitto con questa feature.

## Rischio
Livello: verde
- Costi: nessuna dipendenza né infrastruttura nuova; un endpoint e pochi test.
- Complessità: bassa; l'unico punto delicato è l'escape CSV e la scelta del default della priorità.
- Sicurezza: endpoint in sola lettura su dati in memoria, già esposti in JSON da `GET /api/todos`. Possibile CSV/formula injection se il file viene aperto in Excel con titoli che iniziano con `=`, `+`, `-`, `@` (vedi D6).
- Accoppiamento: se si aggiunge la priorità cambia il contratto di `POST /api/todos` e del JSON dei todo, quindi anche il web (D1, D4).

## Domande aperte
- [ ] D1: Il modello `Todo` oggi non ha una priorità: va aggiunto un campo `Priority` (con validazione in `POST /api/todos`) oppure l'export deve ricavarla in altro modo? — fonti: `requisiti.pdf` e `dati.csv` (colonna `priorita`) vs `api/src/CtbProva.Api/Program.cs` (`Todo(Id, Title, Done)`)
- [ ] D2: Se si aggiunge la priorità, quale valore hanno i todo creati senza priorità (default `media`, `bassa`, oppure priorità obbligatoria con 400)? — fonti: `requisiti.pdf` vs `POST /api/todos` attuale, che accetta solo `title`
- [ ] D3: L'immagine allegata `immagine-1.png` è un quadrato verde di pochi pixel senza schema leggibile: potete rimandare lo schema previsto (colonne, ordine, eventuali campi `id`/`done`)? — fonti: richiesta del thread ("lo schema nell'immagine") vs contenuto di `immagine-1.png`
- [ ] D4: Il web va aggiornato (priorità nel form di creazione, pulsante "Esporta CSV") o la feature riguarda solo l'API? — fonti: regola del `CLAUDE.md` sul contratto API ↔ web vs richiesta del thread, che cita solo l'endpoint
- [ ] D5: Il CSV deve contenere solo `titolo,priorita` come `dati.csv`, o anche `id` e lo stato `done`? — fonti: `dati.csv` vs modello `Todo` (che ha `Id` e `Done`)
- [ ] D6: Serve un header `Content-Disposition: attachment; filename=...` (con quale nome file), il BOM UTF-8 per Excel e la neutralizzazione della formula injection (prefisso `'` sui titoli che iniziano con `=`, `+`, `-`, `@`)? — fonti: `dati.csv` (nessuna indicazione) vs uso tipico di un export CSV
