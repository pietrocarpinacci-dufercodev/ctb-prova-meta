# T-690be0e9 — Piano

Piano provvisorio: mancano PDF, mockup e CSV (vedi D1). Dettaglio in `spec.md`.

## Fonti
- Ricevuti e usati: nessun allegato. PDF, mockup e CSV citati nella richiesta non sono arrivati (`/work/inputs` vuota).
- Codice letto: `api/src/CtbProva.Api/Program.cs`, `api/README.md`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`.
- Regole: `CLAUDE.md` del meta-repo. Nessuna `decisions.md` di altre feature esiste (solo `features/_template`).

## Submodule coinvolti
- api: nuovo `DELETE /api/todos/{id:int}` + `TodoStore.Remove(int)` (su `ConcurrentDictionary.TryRemove`); 204 se rimosso, 404 se assente. Test in `TodoApiTests.cs`. Riga nel README.
- web: `deleteTodo(id, fetchImpl)` in `web/src/todos.js` (stesso schema di `markDone`), pulsante "Elimina" in `app.js` con la conferma/UX dal mockup, test in `web/tests/todos.test.js`.

## Passi
1. Ottenere PDF, mockup e CSV e risolvere D1–D4; aggiornare spec e criteri.
2. api: test rossi per DELETE (esistente → 204 e sparisce da `GET /api/todos`; id assente → 404), poi `TodoStore.Remove` e la route.
3. api: aggiornare `README.md` (tabella endpoint).
4. web: test per `deleteTodo` (URL, metodo DELETE, errore su risposta non ok) con `fetchImpl` finto, poi implementazione.
5. web: pulsante in `app.js` secondo il mockup; per ora riusa `run()` per errori e ridisegno.
6. Dati di esempio: se il CSV arriva, metterlo come fixture nei test e caricare i todo via `POST /api/todos` (la store è in memoria, nessun seed); il parsing resta nei test, non nel codice di prodotto.
7. `dotnet test api` e `npm test --prefix web`.

## Rischio
Livello: giallo
- Costi: bassi, poche decine di righe in due submodule.
- Complessità: bassa nel codice, ma i requisiti sono sconosciuti finché non arrivano gli allegati (conferme, undo, cancellazione multipla potrebbero ampliare lo scope).
- Sicurezza: la cancellazione è distruttiva e senza autenticazione; la CORS dell'API consente qualunque origine e metodo, quindi ogni pagina web può cancellare. Accettabile solo perché è un progetto di prova con dati in memoria.
- Accoppiamento: cambio di contratto tra api e web, da tenere allineato (`CLAUDE.md`); i due PR vanno rilasciati insieme.

## Domande aperte
- [ ] D1: PDF dei requisiti, mockup e CSV non sono arrivati al job: puoi riallegarli nel thread? Senza, i requisiti reali sono ignoti — fonti: richiesta nel thread vs contenuto di `/work/inputs` (vuota)
- [ ] D2: Cancellazione definitiva o "soft" (es. archiviazione/stato cancellato)? Il codice oggi non ha né DELETE né stato oltre `Done` — fonti: richiesta ("cancellazione") vs `Program.cs` (modello `Todo(Id, Title, Done)`)
- [ ] D3: Il pulsante di eliminazione chiede conferma? Si possono cancellare anche i todo già fatti, e c'è una cancellazione multipla o "svuota fatti"? — fonti: mockup (non ricevuto) vs `web/src/app.js`
- [ ] D4: Le righe del CSV hanno colonne diverse da `id,title,done`? Gli id vanno preservati? `POST /api/todos` assegna sempre un nuovo id e crea sempre `done=false` — fonti: CSV (non ricevuto) vs `Program.cs` (`TodoStore.Add`)
