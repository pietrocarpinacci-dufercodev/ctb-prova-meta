# T-b4dd2db2 — Piano

Richiesta iniziale: "cambia DELETE /api/todos/{id} perché risponda 200 con l'elemento rimosso nel corpo".
Aggiornamento nel thread: 404 se l'elemento non viene trovato; nel corpo **non l'oggetto rimosso ma solo l'ID** (sostituisce la parte "elemento rimosso" della richiesta iniziale).

Stato verificato nel codice: `DELETE /api/todos/{id}` **non esiste** (né in `api/src/CtbProva.Api/Program.cs`, né in `api/README.md`, né nei test, né in `web/`). Non c'è quindi un comportamento attuale da cambiare: l'endpoint va creato (confermato, D1).

## Submodule coinvolti
- api: aggiungere `TodoStore.Remove(int id)` (usa `ConcurrentDictionary.TryRemove`, restituisce `bool` o `Todo?`) e `app.MapDelete("/api/todos/{id:int}", ...)` che risponde `200` con nel corpo `{"id": <id>}` (D4; `Results.Ok(new { id })`, con `id` intero come `Todo.Id`) e `404` se l'id non esiste. Aggiornare la tabella endpoint di `api/README.md`. Aggiungere test in `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`: 200 + corpo `{"id": n}` con `n` intero, poi GET sullo stesso id → 404; DELETE su id inesistente → 404.
- web (D3: sì, pulsante di eliminazione con conferma):
  - `web/src/todos.js`: nuova `deleteTodo(id, fetchImpl = fetch)` con `DELETE ${API_BASE}/api/todos/${id}`; se `!response.ok` lancia `errore <status>` (come le altre funzioni); restituisce il JSON `{id}`.
  - `web/src/app.js`: in `render`, accanto a "Fatto", un pulsante "Elimina" per ogni elemento. Al click si apre `window.confirm` con il titolo dell'elemento (es. `Eliminare "<titolo>"? L'operazione non si può annullare.`); solo se l'utente conferma si chiama `run(() => deleteTodo(todo.id))`, che poi ricarica l'elenco; se annulla non succede nulla (nessuna chiamata all'API).
  - `web/tests/todos.test.js`: test di `deleteTodo` (metodo DELETE, URL `/api/todos/<id>`, restituisce `{id}`; errore su risposta non ok, es. 404). La parte con `confirm` sta in `app.js` (DOM) e, come il resto di `app.js`, non è coperta dai test del runner Node.
  - `web/README.md`: nessun cambio necessario (non elenca le funzioni).

## Passi
1. Nessuna domanda aperta: si può approvare.
2. api: implementare `Remove` nello store e l'endpoint DELETE.
3. api: test dei casi 200/corpo con solo ID, 404 dopo rimozione, 404 su id inesistente.
4. api: aggiornare `README.md` (riga DELETE nella tabella).
5. web: aggiungere `deleteTodo` in `todos.js`, il pulsante "Elimina" con `confirm` in `app.js` e i test in `todos.test.js`.
6. Eseguire `dotnet test api` e `npm test --prefix web`.
7. Scrivere `/work/out/api.md`, `/work/out/web.md` e `/work/out/meta.md`.

## Ipotesi (non bloccanti, correggibili nel thread)
- "Popup di conferma" = dialogo nativo del browser `window.confirm` (nessuna dipendenza né markup nuovo), non un modale personalizzato.
- Il pulsante "Elimina" compare su ogni elemento, anche su quelli già fatti.
- Nessun test automatico per il `confirm` (richiederebbe un ambiente DOM; il runner Node non lo ha): da verificare a mano nella pagina.

## Rischio
Livello: verde
- Costi: basso, poche righe e pochi test in due submodule.
- Complessità: bassa; store in memoria, nessuna persistenza.
- Sicurezza: DELETE senza autenticazione con CORS aperto a ogni origine (`AllowAnyMethod`): accettabile in un progetto di prova senza dati reali, ma è la prima operazione distruttiva esposta dall'API; la conferma sta solo nel FE, l'API non la impone.
- Accoppiamento: il contratto dell'API cambia (nuovo endpoint) e va riportato in `web/src/todos.js` e nei test di entrambi (`CLAUDE.md`); i due submodule vanno rilasciati insieme.

## Domande aperte
- [x] D1: DELETE non esiste ancora: va creato? — risposta: sì, creare un nuovo endpoint DELETE (thread)
- [x] D2: Per un id inesistente la risposta è 404? — risposta: sì, 404 se l'elemento non è stato trovato (thread)
- [x] D3: Il web deve avere un pulsante per eliminare? — risposta: sì, aggiungere un pulsante sul FE per eliminare un elemento; al click deve aprirsi un popup di conferma perché l'operazione è distruttiva (thread)
- [x] D4: "Solo l'ID" nel corpo: in che forma? — risposta: oggetto JSON `{"id": 3}`; l'id resta del suo tipo (oggi intero `int`; se fosse un GUID, si riporta il GUID). Il messaggio nel thread sostituisce la richiesta iniziale (elemento rimosso nel corpo).
