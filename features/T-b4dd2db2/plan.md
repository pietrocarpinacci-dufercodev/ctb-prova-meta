# T-b4dd2db2 — Piano

Richiesta iniziale: "cambia DELETE /api/todos/{id} perché risponda 200 con l'elemento rimosso nel corpo".
Aggiornamento nel thread: 404 se l'elemento non viene trovato; nel corpo **non l'oggetto rimosso ma solo l'ID** (sostituisce la parte "elemento rimosso" della richiesta iniziale).

Stato verificato nel codice: `DELETE /api/todos/{id}` **non esiste** (né in `api/src/CtbProva.Api/Program.cs`, né in `api/README.md`, né nei test, né in `web/`). Non c'è quindi un comportamento attuale da cambiare: va creato l'endpoint. Vedi D1.

## Submodule coinvolti
- api: aggiungere `TodoStore.Remove(int id)` (usa `ConcurrentDictionary.TryRemove`, restituisce `bool` o `Todo?`) e `app.MapDelete("/api/todos/{id:int}", ...)` che risponde `200` con nel corpo solo l'ID dell'elemento rimosso (forma da definire, D4) e `404` se l'id non esiste. Aggiornare la tabella endpoint di `api/README.md`. Aggiungere test in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: 200 + corpo con il solo ID, poi GET sullo stesso id → 404; DELETE su id inesistente → 404.
- web: nessuna modifica prevista se la UI non deve cancellare (D3). Se sì: aggiungere la chiamata in `web/src/todos.js`, il pulsante nella pagina e i test (`npm test --prefix web`).

## Passi
1. Risolvere le domande ancora aperte (D1, D3, D4).
2. api: implementare `Remove` nello store e l'endpoint DELETE.
3. api: test dei casi 200/corpo con solo ID, 404 dopo rimozione, 404 su id inesistente.
4. api: aggiornare `README.md` (riga DELETE nella tabella).
5. web: solo se D3 = sì, aggiornare `todos.js`, pagina e test.
6. Eseguire `dotnet test api` (e `npm test --prefix web` se toccato).
7. Scrivere `/work/out/api.md` e `/work/out/meta.md` (e `web.md` se toccato).

## Rischio
Livello: verde
- Costi: basso, poche righe e pochi test in un solo submodule.
- Complessità: bassa; store in memoria, nessuna persistenza.
- Sicurezza: DELETE senza autenticazione con CORS aperto a ogni origine (`AllowAnyMethod`): accettabile in un progetto di prova senza dati reali, ma è la prima operazione distruttiva esposta dall'API.
- Accoppiamento: il contratto dell'API cambia (nuovo endpoint); `CLAUDE.md` impone di riportarlo in `web/src/todos.js` e nei test di entrambi, da qui D3.

## Domande aperte
- [ ] D1: DELETE non esiste ancora: va creato, o si intendeva un'altra branch/versione dell'API in cui già esiste (con risposta diversa, es. 204)? — fonti: richiesta ("cambia DELETE") vs codice (`Program.cs` senza MapDelete)
- [x] D2: Per un id inesistente la risposta è 404? — risposta: sì, 404 se l'elemento non è stato trovato (thread)
- [ ] D3: Il web deve usare DELETE (pulsante "elimina" e test), o basta l'API? — fonti: `CLAUDE.md` (contratto API da riportare in `web/src/todos.js` e nei test di entrambi) vs richiesta (cita solo l'API)
- [ ] D4: "Solo l'ID" nel corpo: oggetto JSON `{ "id": 3 }` o numero nudo `3`? Il messaggio nel thread sostituisce quindi la richiesta iniziale (elemento rimosso nel corpo)? — fonti: richiesta iniziale (elemento rimosso nel corpo) vs messaggio successivo nel thread (solo l'ID); forma non specificata
