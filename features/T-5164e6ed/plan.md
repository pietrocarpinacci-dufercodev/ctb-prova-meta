# T-5164e6ed — Piano

## Submodule coinvolti
- ctb-prova-api (`api`): `Todo` e `NewTodo` in `Program.cs` guadagnano `DateOnly? DueDate`; `TodoStore.Add` accetta la scadenza; validazione di `dueDate` in creazione; nuovo endpoint per i todo scaduti; test in `TodoApiTests.cs`; tabella endpoint in `api/README.md`.
- ctb-prova-web (`web`): `app.js` mostra la data accanto al titolo e la evidenzia se passata; `todos.js` espone una funzione pura `isOverdue(todo, today)`; stile per l'evidenziazione in `index.html`; test in `todos.test.js`; nota in `web/README.md` se serve.

## Passi
1. API, test prima: in `TodoApiTests.cs` aggiungere test per creazione con e senza `dueDate`, lettura di `dueDate`, `dueDate` non valida (400), endpoint scaduti (scaduto, scade oggi, senza data, completato).
2. API: cambiare `record Todo(int Id, string Title, bool Done, DateOnly? DueDate)` e `record NewTodo(string? Title, DateOnly? DueDate)`; `System.Text.Json` gestisce `DateOnly` come `yyyy-MM-dd`. Una data malformata fa fallire il binding con 400 (da verificare con un test, perché con minimal API il body non valido dà 400 senza passare dal gestore).
3. API: `TodoStore.Add(title, dueDate)` e `TodoStore.Overdue(today)`: filtra `DueDate < today`, esclude i completati (salvo D1), ordina per id. Per testare il confine di "oggi" l'oggi si ottiene da `TimeProvider` registrato nel contenitore (già in .NET), sostituibile nei test (vedi D2).
4. API: aggiungere `GET /api/todos/overdue` (vedi D3). Il vincolo `{id:int}` sulla rotta esistente evita il conflitto con `overdue`.
5. API: aggiornare la tabella endpoint di `api/README.md` e `dotnet test api`.
6. Web, contratto (CLAUDE.md del meta-repo): in `todos.js` aggiungere `isOverdue(todo, today)` pura (`!todo.done && todo.dueDate && todo.dueDate < today`, confronto tra stringhe ISO); test in `todos.test.js` (scaduto, oggi, senza data, completato).
7. Web: in `app.js` aggiungere accanto al titolo un `<span>` con la data se presente, con classe `overdue` se `isOverdue`; stile in `index.html`. Usare `textContent`, mai `innerHTML`. Data odierna locale del browser (vedi D2). `npm test --prefix web`.
8. Opzionale e solo se risolta D4: campo data nel form di creazione del web e `addTodo(title, dueDate)`.
9. Il merge va coordinato: API prima, web dopo, perché il web legge `dueDate` (un campo assente è già gestito come "senza scadenza").

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API/web).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/tests/todos.test.js`.
- `features/_template/*`. Non ci sono altre feature con `decisions.md`. Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: piccoli, circa 2 file di codice per submodule più test.
- Complessità: bassa; l'unico punto delicato è la definizione di "oggi" e di fuso orario (D2).
- Sicurezza: nessun dato sensibile; la data è validata dal binding, il web usa `textContent`.
- Accoppiamento: cambia il contratto API/web, ma il campo è facoltativo e additivo, quindi i client esistenti non si rompono.

## Domande aperte
- [ ] D1: i todo già completati con scadenza passata devono comparire tra gli scaduti (e nel web essere evidenziati)? Il piano assume di no — fonti: richiesta nel thread (dubbio del team) vs spec.md
- [ ] D2: "scaduto" è `dueDate` < oggi; "oggi" è la data UTC del server, quella locale del server o quella del browser? Con scadenza uguale a oggi il todo non è scaduto: va bene? — fonti: richiesta nel thread (solo data, nessun fuso) vs api/src/CtbProva.Api/Program.cs
- [ ] D3: nome dell'endpoint dei scaduti: va bene `GET /api/todos/overdue`, oppure `GET /api/todos?overdue=true`? — fonti: richiesta nel thread vs api/README.md
- [ ] D4: nel web serve anche un campo data nel form di creazione? La richiesta cita solo la visualizzazione, quindi il piano lo lascia fuori — fonti: richiesta nel thread vs web/src/app.js
