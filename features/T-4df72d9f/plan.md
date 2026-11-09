# T-4df72d9f — Piano

## Submodule coinvolti
- api: nuovo `GET /api/todos/export` in `src/CtbProva.Api/Program.cs` (CSV costruito a mano, nessuna dipendenza), test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga in `README.md`.
- web: nessuna modifica prevista (nessun pulsante di export richiesto); da rivalutare se D4 lo richiede.

## Passi
1. Risolvere le domande aperte (soprattutto D1–D3) prima di implementare.
2. api, test prima: export con 0 todo (solo intestazione), con 2 todo, con titolo contenente virgola/virgolette/a capo, con titolo che inizia per `=`, e verifica `Content-Type: text/csv`.
3. api: aggiungere `app.MapGet("/api/todos/export", ...)` con `Results.Text(csv, "text/csv")`, colonne secondo D1; escape RFC 4180 e neutralizzazione formula in una piccola funzione locale.
4. api: se D2 conferma il campo priorità, estendere `Todo`/`NewTodo` (default `media`, validazione su `bassa|media|alta`); è un cambio di contratto e va riportato in `web/src/todos.js` e nei test di entrambi (CLAUDE.md).
5. Aggiornare `api/README.md` con il nuovo endpoint.
6. `dotnet test api` (e `npm test --prefix web` se si tocca il web).

## Fonti
- `/work/inputs/requisiti.pdf`: "la priorita ha i valori bassa, media e alta".
- `/work/inputs/dati.csv`: intestazione `titolo,priorita`, righe `Comprare il latte,bassa` e `Preparare la demo,alta`; separatore virgola, nessuna quotatura, header senza accenti.
- `/work/inputs/immagine-1.png`: immagine di circa 1 px di un solo colore, nessuno schema leggibile (vedi D3).
- `CLAUDE.md` del meta-repo; `api/README.md`; `api/src/CtbProva.Api/Program.cs` (modello `Todo(Id, Title, Done)`, rotta `/api/todos/{id:int}`).
- `features/_template` (solo struttura; nessuna altra feature con `decisions.md`).

## Rischio
Livello: verde
- Costi: bassi, un endpoint e pochi test.
- Complessità: bassa; sale se si aggiunge la priorità al modello (D2).
- Sicurezza: export di dati utente in CSV: serve la neutralizzazione della formula injection; nessun segreto coinvolto.
- Accoppiamento: nullo se l'export resta solo in api; la priorità nel modello toccherebbe anche web e i suoi test.

## Domande aperte
- [ ] D1: Quali colonne e quale ordine deve avere il CSV? `dati.csv` ha `titolo,priorita` (senza id né stato), mentre il modello ha `Id, Title, Done` — fonti: dati.csv vs Program.cs
- [ ] D2: Il modello `Todo` non ha la priorità, ma PDF e CSV la prevedono con valori bassa/media/alta: va aggiunta al modello e alla creazione (con quale default?) oppure l'export deve omettere la colonna? — fonti: requisiti.pdf, dati.csv vs api/README.md, Program.cs
- [ ] D3: `immagine-1.png` è praticamente un singolo pixel verde e non contiene alcuno schema leggibile: si può rimandare l'immagine corretta? — fonti: richiesta nel thread vs immagine-1.png
- [ ] D4: Nomi header con o senza accento e in italiano (`titolo,priorita` come in dati.csv) o in inglese come l'API (`title`)? Va incluso il campo `done` ed eventuale pulsante di export nel web? — fonti: dati.csv vs api/README.md
- [ ] D5: Il CSV va servito inline o come download (`Content-Disposition: attachment; filename=todos.csv`)? Fine riga CRLF (RFC 4180) o LF come in dati.csv? — fonti: dati.csv vs RFC 4180
