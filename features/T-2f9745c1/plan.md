# T-2f9745c1 — Piano

## Submodule coinvolti
- api: `GET /api/health` esiste già (`{ "status": "ok" }`, test `Health_returns_ok`). Si estende in modo additivo con `uptimeSeconds` (e `version`, vedi D1); `status` resta invariato. Aggiornare test e `api/README.md`.
- web: nuova sezione "Stato dell'applicazione" in `index.html`/`app.js`, con `fetchHealth` in `todos.js` e test in `tests/todos.test.js`. README del web aggiornato.

## Passi
1. API: in `Program.cs` far restituire a `/api/health` anche `uptimeSeconds` (calcolato dall'avvio del processo) e `version` (assembly). Nessun nuovo endpoint, nessuna dipendenza.
2. API: test in `TodoApiTests.cs` per `status == "ok"` e `uptimeSeconds >= 0`; aggiornare la riga/tabella in `api/README.md`.
3. Web: `fetchHealth(fetchImpl)` in `web/src/todos.js` (stesso stile di `fetchTodos`: errore se `!response.ok`).
4. Web: in `index.html` un blocco `<section id="health">` con badge di stato; in `app.js` chiamata all'avvio e ogni 30 s, con stato "non raggiungibile" (rosso) se la chiamata fallisce, "ok" (verde) altrimenti; stile in `style.css`. Il badge usa testo, non solo colore.
5. Web: test con fetch finto per `fetchHealth` (ok e errore); funzione pura per l'etichetta di stato, testata con `node --test`.
6. Verifica: `dotnet test api` e `npm test --prefix web`.

## Fonti
- `CLAUDE.md` del meta-repo (contratto API↔web: un cambio all'API va riportato in `web/src/todos.js` e nei test di entrambi).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/tests/todos.test.js`.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: poche decine di righe in due submodule.
- Complessità: bassa; nessuna nuova dipendenza né endpoint.
- Sicurezza: l'endpoint è pubblico (CORS aperto): niente informazioni sensibili nel payload (no connection string, no path, no versioni di runtime).
- Accoppiamento: la modifica è additiva, quindi il contratto resta compatibile; vanno però aggiornati API e web insieme (due PR).

## Domande aperte
- [ ] D1: `/api/health` esiste già e restituisce solo `status: ok`. Basta estenderlo con `uptimeSeconds` e `version`, o serve altro (es. controllo dello store, dipendenze esterne)? — fonti: richiesta nel thread ("sviluppami una api") vs `Program.cs` esistente
- [ ] D2: la pagina di stato va nella stessa pagina delle cose da fare (piano attuale) o in una pagina separata `health.html`? — fonti: richiesta nel thread ("una pagina di FE") vs `web/src/index.html`
- [ ] D3: l'aggiornamento automatico ogni 30 s va bene, o basta il caricamento iniziale più un pulsante "Aggiorna"? — fonti: richiesta nel thread (non specificato)
