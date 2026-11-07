# T-0e605945 — Piano

## Fonti
- Allegati: `requisiti-cancellazione 3.pdf` (R1–R6), `immagine-1.png` (mockup della conferma), `todos-esempio 3.csv` (dati di test), `immagine-2.png` (screenshot del thread Teams: ripete il mockup in piccolo e il riquadro di conferma con bordo e sfondo salmone; non aggiunge requisiti nuovi).
- Repo: `CLAUDE.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`, `web/src/todos.js`, `web/src/app.js`, `web/src/index.html`, `web/src/style.css`, `web/tests/todos.test.js`, `features/_template/*`.
- Spec e decisioni: `features/T-0e605945/spec.md`, `features/T-0e605945/decisions.md`.

## Submodule coinvolti
- api: `TodoStore.Remove(int id)` e `DELETE /api/todos/{id:int}` (204/404) in `Program.cs`; test; riga nuova nella tabella di `README.md`. CORS già consente ogni metodo.
- web: `deleteTodo(id, fetchImpl)` in `src/todos.js`; pulsante "Elimina" e riquadro di conferma in `src/app.js` e `src/index.html`; stile in `src/style.css`; test in `tests/todos.test.js`.

## Passi
1. api: aggiungere `Remove(id)` a `TodoStore` (`_items.TryRemove`) e `app.MapDelete("/api/todos/{id:int}", ...)` → `Results.NoContent()` o `Results.NotFound()`.
2. api: copiare il CSV in `api/tests/CtbProva.Api.Tests/todos-esempio.csv`, con tutte e 4 le righe (copia nel build output dal `.csproj` dei test). Helper di test che legge le righe e crea i todo via `POST` (e `/done` se `done=true`; gli id li assegna lo store, quindi i test usano gli id restituiti, non quelli del CSV).
3. api: test `DELETE` esistente → 204 e poi `GET` 404; todo completato (riga 2 del CSV) → 204; id inesistente → 404; doppia cancellazione → 204 poi 404; gli altri todo restano.
4. api: aggiornare `README.md` con la riga `DELETE /api/todos/{id}`.
5. web: `deleteTodo(id, fetchImpl = fetch)` come `markDone`: `DELETE`, errore se `!ok`, nessun body da leggere (204).
6. web: in `render`, un pulsante "Elimina" per ogni todo, anche completato. Il pulsante "Fatto" resta com'è (solo per i todo aperti) e la casella del mockup non si implementa (D1).
7. web: riquadro di conferma in pagina (D2), un solo elemento in `index.html` sotto la lista, nascosto di default e non modale. Testo `Eliminare "<titolo>"?` impostato con `textContent`, mai `innerHTML`, e due pulsanti: "Annulla" lo nasconde senza chiamare l'API, "Elimina" chiama `run(() => deleteTodo(id))`, che ricarica la lista (R5), mostra gli errori e nasconde il riquadro. Se si preme "Elimina" su un altro todo, il riquadro si aggiorna sul nuovo titolo.
8. web: stile in `style.css` come nel mockup: pulsanti "Elimina" arancio con testo bianco e angoli arrotondati, riquadro con bordo e sfondo salmone chiaro e testo del titolo in evidenza. Nessun colore o testo extra, tipo "(conferma prima di cancellare)", che nel mockup è solo una didascalia.
9. web: copiare il CSV in `web/tests/todos-esempio.csv`, 4 righe; test di `deleteTodo` (URL, metodo `DELETE`, errore su 404) con `fetch` finto, e test su `sortTodos`/`summary` con i dati del CSV letti con `node:fs`.
10. Contratto: il cambio va riportato in `web/src/todos.js` e nei test di entrambi (`CLAUDE.md`). Eseguire `dotnet test api` e `npm test --prefix web`.
11. Commit locali `[T-0e605945] ...` su `claude/T-0e605945` in ogni submodule toccato; descrizioni PR in `/work/out/api.md`, `/work/out/web.md`, `/work/out/meta.md`. La logica del riquadro è in `app.js` (DOM) e non ha test automatici: va indicato nelle PR.

## Copertura requisiti
Ricontrollata rispetto a `requisiti-cancellazione 3.pdf`, `immagine-1.png` e `todos-esempio 3.csv`: nessun requisito manca.

| Req. | Dove nel piano | Verifica |
| --- | --- | --- |
| R1 DELETE 204/404 | passi 1, 3 | test API (esistente → 204, inesistente → 404) |
| R2 completato eliminabile | passo 3 (riga 2 del CSV, `done=true`) | test API |
| R3 "Elimina" accanto a ogni todo | passo 6 | solo a mano: `app.js` non ha test DOM |
| R4 conferma come da mockup | passi 7, 8 | solo a mano: nessun test DOM |
| R5 lista aggiornata senza ricaricare | passo 7 (`run` → `fetchTodos`) | solo a mano; `deleteTodo` coperto dai test web |
| R6 dati di esempio nel CSV | passi 2, 9 | i test di entrambi i submodule leggono `todos-esempio.csv` |
| Fuori ambito (multipla, cestino, undo) | non pianificati | — |

Note: R3–R5 non hanno test automatici per via dell'assenza di test DOM nel web; un test DOM (jsdom) sarebbe una dipendenza nuova e non è pianificato. Il nome `todos-esempio.csv` di R6 differisce dall'allegato `todos-esempio 3.csv`: già risolto in D3.

## Rischio
Livello: verde
- Costi: piccoli, circa 5 file di codice e 2 di test; nessuna dipendenza nuova.
- Complessità: bassa; si segue il modello di `markDone` e `TodoStore`. Unica parte con un po' di UI è il riquadro, senza test DOM.
- Sicurezza: la cancellazione è irreversibile, ma i dati sono in memoria e di prova; la conferma è lato client. Il titolo nel riquadro va inserito con `textContent` (no XSS). CORS già aperto a ogni origine, invariato.
- Accoppiamento: contratto API ↔ `web/src/todos.js`, da cambiare in entrambi i submodule; ordine di merge indifferente (il web finché l'API non ha DELETE riceve 404/405 e mostra l'errore).

## Domande aperte
- [x] D1: Il mockup mostra una casella di spunta per "fatto", mentre la UI attuale ha il pulsante "Fatto" solo per i todo aperti. Sostituire o lasciare? — risposta: si lascia il pulsante "Fatto" com'è; la casella del mockup non si implementa.
- [x] D2: Conferma con riquadro in pagina o `window.confirm`? Cosa significa "annulla" fuori ambito? — risposta: riquadro in pagina come nel mockup, con "Annulla" che chiude solo la conferma; "annulla" fuori ambito nel PDF è l'undo dopo la cancellazione.
- [x] D3: Copiare il CSV come fixture, senza seed, con tutte e 4 le righe? — risposta: sì, copiarlo come fixture con il nome `todos-esempio.csv` e tenere tutte e 4 le righe.
