# T-abc99dff — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `Program.cs` (CSV), test in `TodoApiTests.cs`, riga nella tabella endpoint di `README.md`. Se D1 porta a un campo `priority` nel modello, cambia anche `Todo`/`NewTodo`/`TodoStore`.
- web: nessuna modifica prevista (l'export non è richiesto nella pagina); da rivalutare solo se D1 cambia il contratto di `/api/todos`.

## Passi
1. Risolvere le domande aperte D1–D4 (bloccanti per la forma del CSV).
2. Test prima (TDD) in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: status 200, `Content-Type: text/csv`, intestazione `titolo,priorita`, una riga per todo, escape di virgole/virgolette/a-capo nel titolo, elenco vuoto = solo intestazione.
3. Aggiungere in `Program.cs` `app.MapGet("/api/todos/export", ...)` che costruisce il CSV dai todo di `store.All()` e risponde con `Results.Text(csv, "text/csv")`. Nessuna conflittualità con `/api/todos/{id:int}`: il vincolo `:int` esclude `export`.
4. Escape RFC 4180 dei campi (virgolette raddoppiate, campo tra virgolette se contiene `,` `"` o a-capo) con una piccola funzione locale; nessuna dipendenza nuova.
5. Se D1 = aggiungere la priorità: estendere `Todo` con `Priority` (valori `bassa|media|alta`, default da D2), validazione in POST (400 se fuori elenco) e riportare il cambio in `web/src/todos.js` e nei test di entrambi, come da CLAUDE.md.
6. Aggiornare `api/README.md` (tabella endpoint) e lanciare `dotnet test api`.

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: «la priorita ha i valori bassa, media e alta».
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`; righe `Comprare il latte,bassa` e `Preparare la demo,alta`; separatore virgola, nomi colonna senza accenti, nessuna colonna id/fatto.
- Allegato `/work/inputs/immagine-1.png`: immagine di circa 1 KB, praticamente un unico pixel verde; **nessuno schema leggibile** (vedi D3).
- `/work/repo/CLAUDE.md` (contratto API/web, comandi di test).
- `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs` (modello attuale: `Todo(Id, Title, Done)`).
- `features/_template/`; non ci sono `decisions.md` di altre feature.

## Rischio
Livello: verde
- Costi: basso, un endpoint di sola lettura e pochi test.
- Complessità: bassa; sale un po' se D1 introduce il campo priorità (modello, validazione, web).
- Sicurezza: CSV injection se un titolo inizia con `=`, `+`, `-`, `@` e il file è aperto in un foglio di calcolo (D4); nessun segreto né dato reale coinvolto.
- Accoppiamento: la priorità, se aggiunta, tocca il contratto api↔web.

## Domande aperte
- [ ] D1: `Todo` non ha il campo priorità, ma `dati.csv` ha la colonna `priorita` e il PDF ne definisce i valori. Si aggiunge `priority` al modello (e a POST/web) o la colonna esportata resta vuota? — fonti: dati.csv e requisiti.pdf vs `Program.cs` (record `Todo`)
- [ ] D2: se la priorità viene aggiunta, qual è il default per i todo creati senza priorità (es. `media`) e il POST deve rifiutare valori fuori da bassa/media/alta? — fonti: requisiti.pdf vs `Program.cs` (POST accetta solo `title`)
- [ ] D3: l'immagine allegata è un pixel verde e non contiene alcuno schema. Si può rinviare l'allegato corretto? Nel frattempo si assume per il CSV lo schema di `dati.csv` (`titolo,priorita`). — fonti: richiesta nel thread vs immagine-1.png
- [ ] D4: le colonne `id` e `done` vanno esportate? `dati.csv` ha solo `titolo,priorita`. E i titoli che iniziano con `=`, `+`, `-`, `@` vanno neutralizzati (prefisso `'`)? — fonti: dati.csv vs modello `Todo`
