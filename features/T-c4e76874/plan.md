# T-c4e76874 — Piano

## Submodule coinvolti
- api: nuovo `GET /api/todos/export` in `src/CtbProva.Api/Program.cs` che restituisce i todo in CSV; test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`; riga nuova nella tabella endpoint di `README.md`. In base alla risposta a D1 può servire anche il campo `priority` su `Todo`/`NewTodo`.
- web: nessuna modifica prevista (nessun pulsante di export richiesto, vedi D5).

## Passi
1. (api, test per primo) Test su `/api/todos/export`: status 200, `Content-Type: text/csv`, riga di intestazione, una riga per todo, escaping di virgole, virgolette e a capo nel titolo.
2. (api) Aggiungere in `Program.cs` `app.MapGet("/api/todos/export", ...)` che costruisce il CSV da `store.All()` con `Results.Text(csv, "text/csv")`. La rotta resta distinta da `/api/todos/{id:int}` grazie al vincolo `:int`.
3. (api) Escaping RFC 4180 con una piccola funzione locale: campo tra virgolette se contiene `,`, `"` o a capo, e `"` raddoppiate. Questo vale anche per proteggere dalle formule Excel (D5).
4. (api) Colonne e valori secondo le risposte a D1, D2 e D3; per ora la bozza segue `dati.csv` (`titolo,priorita`).
5. (api) Aggiornare `README.md` con il nuovo endpoint.
6. Contratto: se cambia la forma di `Todo` (D1), riportarlo in `web/src/todos.js` e nei test del web, come richiede il CLAUDE.md. Altrimenti il web non cambia.
7. Eseguire `dotnet test api` (e `npm test --prefix web` se il web è toccato).

## Fonti
- Allegato `/work/inputs/dati.csv`: intestazione `titolo,priorita`, separatore virgola, due righe (`Comprare il latte,bassa` e `Preparare la demo,alta`). Non ha le colonne `id` e `done`.
- Allegato `/work/inputs/requisiti.pdf`: una riga, «la priorita ha i valori bassa, media e alta».
- Allegato `/work/inputs/immagine-1.png`: immagine di circa 1 KB, che nel job risulta quasi vuota (un riquadro uniforme verde chiaro). Non contiene uno schema leggibile, vedi D3.
- `api/src/CtbProva.Api/Program.cs`: `Todo(int Id, string Title, bool Done)`, nessun campo priorità; `TodoStore.All()` ordina per Id.
- `api/README.md` e `CLAUDE.md` del meta-repo (contratto API/web, comandi di test).
- `features/_template/`: nessun'altra feature esistente, quindi nessun confronto con altre `decisions.md`.

## Rischio
Livello: verde
- Costi: basso, un endpoint di sola lettura e qualche test.
- Complessità: bassa; l'unica parte delicata è l'escaping del CSV. Cresce solo se D1 introduce il campo priorità (modello, POST, web).
- Sicurezza: endpoint pubblico, senza autenticazione e con CORS aperto come gli altri; esporta solo dati di prova. Rischio di CSV injection sui titoli (D5).
- Accoppiamento: basso se `Todo` non cambia; medio se si aggiunge `priority` (API, `web/src/todos.js`, test di entrambi).

## Domande aperte
- [ ] D1: `Todo` non ha il campo priorità, ma `dati.csv` e il PDF la prevedono. Va aggiunta al modello (e a `POST /api/todos`, con default?) oppure l'export deve avere solo le colonne esistenti? — fonti: dati.csv + requisiti.pdf vs api/src/CtbProva.Api/Program.cs
- [ ] D2: Quali colonne e in che ordine? `dati.csv` ha solo `titolo,priorita`, mentre il modello ha anche `id` e `done`. Le intestazioni restano in italiano senza accento (`titolo`, `priorita`) anche se l'API usa nomi in inglese? — fonti: dati.csv vs api/README.md
- [ ] D3: `immagine-1.png` non contiene uno schema leggibile (quasi vuota). Potete rimandarla o descrivere lo schema a parole? Finché manca, nessun requisito viene preso dall'immagine. — fonti: immagine-1.png vs richiesta nel thread
- [ ] D4: Quale priorità hanno i todo esistenti o creati senza priorità, e quale valore va esportato (`media` come default)? Va validato che sia uno tra bassa, media e alta? — fonti: requisiti.pdf vs api/src/CtbProva.Api/Program.cs
- [ ] D5: Servono `Content-Disposition: attachment; filename=...`, la codifica (UTF-8 con o senza BOM, a capo `\r\n`) e la neutralizzazione dei titoli che iniziano con `=`, `+`, `-` o `@`? Va aggiunto un pulsante di download nel web? — fonti: dati.csv vs richiesta nel thread
