# T-82b364c6 — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `Program.cs`, test in `TodoApiTests.cs`, riga nella tabella endpoint di `README.md`.
- web: nessuna modifica prevista (nessun pulsante di export richiesto), salvo risposta a D6.

## Passi
1. (Dopo le risposte a D1–D4) test in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: 200 + `text/csv`, intestazione, righe in ordine di Id, elenco vuoto, quoting RFC 4180, neutralizzazione formula.
2. Se D1 conferma la priorità: aggiungere `Priority` (bassa|media|alta) a `Todo`/`NewTodo`/`TodoStore`, con validazione e default; riportare il cambio di contratto in `web/src/todos.js` e nei test web (regola del CLAUDE.md).
3. Implementare l'endpoint in `Program.cs` con `Results.Text(csv, "text/csv")`, scrivendo il CSV a mano (nessuna dipendenza nuova) con un piccolo helper di escaping.
4. Aggiornare `api/README.md`.
5. Eseguire `dotnet test api` (e `npm test --prefix web` se il web cambia).

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: "la priorita ha i valori bassa, media e alta".
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, righe `Comprare il latte,bassa` e `Preparare la demo,alta`; separatore virgola, nessun id/done.
- Allegato `/work/inputs/immagine-1.png`: PNG di circa 1 px, tutto verde; non contiene alcuno schema leggibile (vedi D3).
- `api/src/CtbProva.Api/Program.cs`: `Todo(Id, Title, Done)`, nessun campo priorità.
- `api/README.md`, `CLAUDE.md` del meta-repo, `features/T-9dfe1d36/decisions.md` (nessun conflitto).

## Rischio
Livello: verde
- Costi: basso, un endpoint e pochi test.
- Complessità: bassa; sale a media se si introduce la priorità (D1), perché tocca il contratto API e il web.
- Sicurezza: formula injection nel CSV (D5); nessun segreto coinvolto.
- Accoppiamento: l'aggiunta di `Priority` cambierebbe il contratto con `web/src/todos.js`.

## Domande aperte
- [ ] D1: `Todo` non ha il campo priorità, ma `dati.csv` e il PDF lo richiedono: va aggiunto a modello e API (con default?) o esportato un valore fisso? — fonti: requisiti.pdf, dati.csv vs Program.cs
- [ ] D2: `dati.csv` ha solo `titolo,priorita`, mentre il modello ha anche `Id` e `Done`: l'export deve avere solo quelle due colonne o anche id e fatto? — fonti: dati.csv vs Program.cs
- [ ] D3: l'immagine allegata è un PNG di circa 1 px, tutto verde, senza schema leggibile: puoi ricaricarla o descrivere lo schema? — fonti: immagine-1.png vs richiesta
- [ ] D4: quale priorità per i todo creati senza priorità (default `media`?) e quale risposta per un valore fuori da bassa/media/alta (400)? — fonti: requisiti.pdf
- [ ] D5: i titoli che iniziano con `= + - @` vanno neutralizzati (prefisso `'`) per evitare la formula injection in Excel? — fonti: spec.md vs dati.csv
- [ ] D6: serve un pulsante di export nel web o basta l'endpoint? — fonti: richiesta vs CLAUDE.md (contratto api/web)
