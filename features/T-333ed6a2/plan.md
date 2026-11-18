# T-333ed6a2 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `src/CtbProva.Api/Program.cs`; campo priorità sul modello `Todo` e su `NewTodo` se D2 lo conferma; test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`; tabella degli endpoint in `README.md`.
- web: solo se D2 o D5 lo richiedono (priorità nel form di creazione in `src/todos.js`/`src/app.js`, link "Esporta CSV"), con i relativi test.

## Passi
1. Test che falliscono (xUnit, `WebApplicationFactory<Program>` come i test esistenti): risposta 200, `Content-Type` `text/csv; charset=utf-8`, intestazione `titolo,priorita`, una riga per cosa da fare in ordine di `Id`, solo intestazione con lista vuota, quoting RFC 4180 di un titolo con virgola o doppi apici, neutralizzazione delle formule (D6), `GET /api/todos/{id}` ancora a posto.
2. Se D2 conferma la priorità: aggiungere al record `Todo` un campo `Priority` con i valori `bassa|media|alta` e un valore di default (D3), accettarlo come opzionale in `NewTodo` con validazione 400 per i valori fuori elenco, e aggiungere i test.
3. In `Program.cs` mappare `app.MapGet("/api/todos/export", ...)`, che costruisce il CSV da `store.All()` con `StringBuilder` e un piccolo helper di escape (nessuna dipendenza nuova) e restituisce `Results.Text(csv, "text/csv; charset=utf-8")`. La rotta letterale non entra in conflitto con `{id:int}`.
4. Aggiornare la tabella degli endpoint in `api/README.md`.
5. Web, solo se D2 o D5 lo richiedono: allineare `web/src/todos.js` (ed eventualmente `app.js`) e i test, come chiede il `CLAUDE.md` del meta-repo per i cambi di contratto.
6. Verifica: `dotnet test api` e, se il web cambia, `npm test --prefix web`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: valori di priorità `bassa`, `media`, `alta`.
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore `,`, fine riga LF, senza BOM visibile.
- Allegato `/work/inputs/immagine-1.png`: immagine tutta verde senza contenuto leggibile, quindi non usata (D1).
- `api/src/CtbProva.Api/Program.cs`: modello `Todo(Id, Title, Done)` senza priorità, `TodoStore.All()` ordinato per `Id`, rotta `/api/todos/{id:int}`.
- `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: stile dei test di integrazione.
- `api/README.md`: tabella degli endpoint.
- `CLAUDE.md` del meta-repo: un cambio di contratto va riportato in `web/src/todos.js` e nei test di entrambi i submodule.
- `features/T-9dfe1d36/decisions.md`: execute delegato a `backend-dev`, nessuna decisione in conflitto con questa feature.

## Rischio
Livello: giallo
- Costi: bassi, un endpoint e pochi test; nessuna dipendenza nuova.
- Complessità: bassa per il solo export. Diventa media se va aggiunta la priorità al modello (D2), perché cambia il contratto di `POST /api/todos` e della risposta JSON.
- Sicurezza: export senza autenticazione, come gli altri endpoint (CORS aperto). Rischio di iniezione di formule CSV quando il file si apre in Excel (D6).
- Accoppiamento: se il modello cambia, servono modifiche coordinate tra api e web. I requisiti sono incompleti finché l'immagine resta illeggibile (D1).

## Domande aperte
- [ ] D1: `immagine-1.png` è un'immagine tutta verde senza schema leggibile: potete ricaricare lo schema o descriverlo nel thread (colonne, ordine, tipi)? — fonti: richiesta del thread ("lo schema nell'immagine") vs allegato `immagine-1.png`
- [ ] D2: il modello `Todo` non ha la priorità, che invece compare in `dati.csv` e in `requisiti.pdf`: va aggiunta al modello e a `POST /api/todos` (contratto API e web)? Oppure l'export deve scrivere una priorità fissa o vuota? — fonti: `dati.csv`/`requisiti.pdf` vs `api/src/CtbProva.Api/Program.cs`
- [ ] D3: se si aggiunge la priorità, qual è il valore di default per le cose da fare create senza priorità (per esempio `media`)? — fonti: `requisiti.pdf` vs `POST /api/todos` attuale (solo `title`)
- [ ] D4: il CSV deve avere solo le colonne `titolo,priorita` di `dati.csv`, oppure anche `id` e lo stato `done` che l'API già gestisce? — fonti: `dati.csv` vs modello `Todo(Id, Title, Done)`
- [ ] D5: serve anche un pulsante o link "Esporta CSV" nella pagina web, oppure l'ambito è solo l'API? — fonti: richiesta del thread (solo endpoint) vs `CLAUDE.md` del meta-repo (cambi di contratto riportati nel web)
- [ ] D6: come va trattata l'iniezione di formule (titoli che iniziano con `=`, `+`, `-`, `@`)? Proposta: prefisso `'`, che però modifica il dato esportato. In alternativa si lascia il titolo invariato. Vanno decisi anche `Content-Disposition: attachment; filename="todos.csv"` e il BOM UTF-8 per Excel. — fonti: `dati.csv` (nessuna indicazione) vs spec.md (criterio sull'iniezione di formule)
