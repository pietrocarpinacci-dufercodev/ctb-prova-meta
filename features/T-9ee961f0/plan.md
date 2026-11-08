# T-9ee961f0 — Piano

## Submodule coinvolti
- ctb-prova-api (`api`): nuovo endpoint `GET /api/todos/export` in `Program.cs` che restituisce i todo in CSV; test in `TodoApiTests.cs`; riga nuova nella tabella di `api/README.md`.

(`web` non cambia: nessun contratto esistente viene modificato. Se D3 richiede un pulsante di download, si aggiunge `web/src/todos.js`.)

## Passi
1. Risolvere le domande aperte (soprattutto D1, D2, D3) prima di implementare.
2. Test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: status 200, `Content-Type: text/csv`, intestazione, una riga per todo, escape di virgole/virgolette/a capo nel titolo, elenco vuoto = solo intestazione.
3. In `Program.cs` mappare `GET /api/todos/export` con `Results.Text(csv, "text/csv")` (valutare `Results.File` con `Content-Disposition: attachment; filename=todos.csv`). La rotta statica `export` non confligge con `/api/todos/{id:int}`, grazie al vincolo `int`.
4. Costruire il CSV a mano in una piccola funzione: intestazione `titolo,priorita` come in `dati.csv`, una riga per todo da `store.All()`, campi tra virgolette doppie se contengono `,` `"` o a capo. Mitigare la CSV injection prefissando con `'` i valori che iniziano con `=`, `+`, `-`, `@` (vedi D4).
5. Se D1 conferma che serve la priorità: aggiungere `Priority` a `Todo`/`NewTodo` (valori `bassa|media|alta`, validazione 400 altrimenti, default da decidere), poi aggiornare README, `web/src/todos.js` e i test di entrambi i submodule, come richiesto dal CLAUDE.md per i cambi di contratto.
6. `dotnet test api` (e `npm test --prefix web` se il web cambia).

## Fonti
- `/work/inputs/requisiti.pdf`: unica frase, "la priorita ha i valori bassa, media e alta".
- `/work/inputs/dati.csv`: intestazione `titolo,priorita`, righe `Comprare il latte,bassa` e `Preparare la demo,alta`; separatore virgola, nessuna virgoletta, nessuna colonna id/done.
- `/work/inputs/immagine-1.png`: immagine di circa 1 px, verde, senza testo né schema leggibile. Non ho potuto ricavarne alcun requisito (vedi D2).
- `api/src/CtbProva.Api/Program.cs`: il record `Todo(Id, Title, Done)` non ha il campo priorità.
- `api/README.md` e `CLAUDE.md` del meta-repo: elenco degli endpoint e regola sul contratto API/web.

## Rischio
Livello: verde
- Costi: basso, una rotta e pochi test, nessuna dipendenza nuova.
- Complessità: bassa se si esporta solo ciò che esiste. Sale a media se serve aggiungere la priorità al modello (D1).
- Sicurezza: dati in memoria e senza autenticazione, come gli altri endpoint. Serve l'escape del CSV e la mitigazione della CSV injection sui titoli inseriti dagli utenti.
- Accoppiamento: basso per l'export. Aggiungere `Priority` cambia il contratto e tocca anche `web` (CLAUDE.md).

## Domande aperte
- [ ] D1: Il modello `Todo` non ha il campo priorità, ma `dati.csv` e il PDF la richiedono. Va aggiunta `priority` (bassa|media|alta) a creazione e modello, o l'export deve omettere la colonna? — fonti: requisiti.pdf e dati.csv vs `Program.cs` e `api/README.md`
- [ ] D2: L'immagine allegata è di circa 1 px, verde, e non contiene uno schema leggibile. Si può rimandarla o descrivere lo schema a parole? — fonti: richiesta nel thread vs immagine-1.png
- [ ] D3: Le colonne esatte e l'ordine sono solo `titolo,priorita` come in `dati.csv`, o servono anche `id` e `done` come nel JSON? Serve un pulsante di download nel web? — fonti: dati.csv vs contratto `/api/todos`
- [ ] D4: Quale default di priorità hanno i todo esistenti e quelli creati senza priorità (un `POST` con solo `title`)? Si accettano priorità non valide con 400? — fonti: requisiti.pdf vs `POST /api/todos` attuale
- [ ] D5: Intestazione con `titolo,priorita` senza accento (come nel file) e separatore virgola con fine riga `\n` sono corretti? Serve `Content-Disposition: attachment`? — fonti: dati.csv vs convenzioni dell'API
