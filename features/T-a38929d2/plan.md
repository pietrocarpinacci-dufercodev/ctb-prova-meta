# T-a38929d2 — Piano

## Fonti
Usate:
- Richiesta del thread.
- `api/src/CtbProva.Api/Program.cs`: oggi esistono solo GET, POST e `POST /done`. Lo store è in memoria (`TodoStore`, `ConcurrentDictionary`).
- `api/README.md`: tabella degli endpoint da aggiornare.
- `web/src/todos.js`, `web/src/app.js`: client dell'API e rendering (pulsante "Fatto" per ogni riga).
- `CLAUDE.md` del meta-repo: un cambio di contratto va riportato in API, `web/src/todos.js` e nei test di entrambi.

Citate nella richiesta ma **non ricevute** (`/work/inputs` vuoto, nessun file nel repo): PDF dei requisiti, mockup incollato, CSV dei dati di esempio. I passi qui sotto sono provvisori e dipendono da D1–D3.

## Submodule coinvolti
- api: `DELETE /api/todos/{id}` (`TodoStore.Remove`), test in `TodoApiTests.cs`, riga nel README.
- web: `deleteTodo(id)` in `todos.js`, pulsante di cancellazione in `app.js`/`index.html` come da mockup, test in `todos.test.js`.

## Passi
1. Ottenere PDF, mockup e CSV (D1–D3) e riscrivere i criteri di `spec.md` su di essi.
2. api: aggiungere `TodoStore.Remove(int id)` (`TryRemove`) e `app.MapDelete("/api/todos/{id:int}")`: 204 se rimosso, 404 se assente (salvo D4/D5).
3. api: test per cancellazione riuscita, 404 su id inesistente, todo assente da `GET /api/todos` dopo la cancellazione. Se il CSV arriva, usarlo come dati di esempio (D3).
4. api: aggiungere la riga `DELETE` alla tabella di `api/README.md`.
5. web: `deleteTodo(id, fetchImpl)` in `web/src/todos.js`, sullo stesso schema di `markDone`.
6. web: pulsante di cancellazione per riga in `app.js`, con l'aspetto del mockup. Eventuale conferma secondo D6.
7. web: test di `deleteTodo` in `web/tests/todos.test.js` (successo ed errore).
8. Verifica: `dotnet test api` e `npm test --prefix web`.

## Rischio
Livello: giallo
- Costi: bassi; due submodule e poche righe ciascuno. Il livello è giallo perché i requisiti non sono ancora noti.
- Complessità: bassa; i dati sono in memoria e non c'è persistenza.
- Sicurezza: la cancellazione è distruttiva e irreversibile, e l'API non ha autenticazione e ha CORS aperto a ogni origine (`AllowAnyOrigin`, `AllowAnyMethod`). Chiunque raggiunga l'API può cancellare tutto. Accettabile solo perché è un progetto di prova senza clienti reali (D7).
- Accoppiamento: alto fra api e web, perché il contratto è condiviso; i due branch `claude/T-a38929d2` vanno mantenuti allineati.

## Domande aperte
- [ ] D1: Il PDF dei requisiti non è arrivato al job: può essere riallegato? Senza, i criteri di accettazione sono solo ipotesi — fonti: richiesta del thread vs `/work/inputs` (vuoto)
- [ ] D2: Il mockup incollato non è arrivato (nessuna immagine né testo): può essere riallegato? Posizione e aspetto del controllo di cancellazione sono sconosciuti — fonti: richiesta del thread vs `web/src/index.html` e `web/src/app.js`
- [ ] D3: Il CSV di esempio non è arrivato: può essere riallegato? Serve per sapere colonne e righe da usare nei test — fonti: richiesta del thread vs `/work/inputs` (vuoto)
- [ ] D4: Quale risposta deve dare `DELETE` in caso di successo, 204 senza corpo o 200 col todo cancellato? — fonti: requisito ambiguo; `api/src/CtbProva.Api/Program.cs` (`/done` restituisce 200 col todo)
- [ ] D5: La cancellazione di un id già cancellato o inesistente deve dare 404 o 204 (idempotente)? — fonti: requisito ambiguo vs `GET /api/todos/{id}` (404 se assente)
- [ ] D6: Serve una conferma prima di cancellare dalla pagina, o il click cancella subito? — fonti: requisito ambiguo, probabilmente nel mockup
- [ ] D7: La cancellazione resta senza autenticazione, con CORS aperto a ogni origine, come il resto dell'API? — fonti: richiesta vs `api/src/CtbProva.Api/Program.cs` (CORS `AllowAnyOrigin`) e regole di sicurezza del `CLAUDE.md`
