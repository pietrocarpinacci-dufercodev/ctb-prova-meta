# T-5164e6ed — Piano

## Submodule coinvolti
- ctb-prova-api (`api`): `Todo` e `NewTodo` in `Program.cs` guadagnano `DateOnly? DueDate`; `TodoStore.Add` accetta la scadenza; nuovo `GET /api/todos/overdue`; test in `TodoApiTests.cs`; tabella endpoint in `api/README.md`.
- ctb-prova-web (`web`): `todos.js` espone `isOverdue(todo, today)` e `addTodo(title, dueDate)`; `app.js` mostra la data accanto al titolo, la evidenzia se scaduta e legge il nuovo campo data del form; `index.html` ha il campo `<input type="date">` facoltativo e lo stile `overdue`; test in `todos.test.js`.

## Passi
1. API, test prima: in `TodoApiTests.cs` aggiungere test per creazione con e senza `dueDate`, lettura di `dueDate` (lista, singolo, `done`), `dueDate` non valida (400), `GET /api/todos/overdue` (scaduto, scade oggi, senza data, completato escluso).
2. API: `record Todo(int Id, string Title, bool Done, DateOnly? DueDate)` e `record NewTodo(string? Title, DateOnly? DueDate)`; `System.Text.Json` gestisce `DateOnly` come `yyyy-MM-dd`. Una data malformata fa fallire il binding con 400 (da verificare con un test).
3. API: `TodoStore.Add(title, dueDate)` e `TodoStore.Overdue(today)`: `!Done && DueDate < today`, ordinati per `DueDate` e, a parità, per id (richiesta di followup). "Oggi" è la data UTC del server, da `TimeProvider` (registrato nel contenitore, sostituibile nei test per provare il confine di "scade oggi").
4. API: `GET /api/todos/overdue`. Il vincolo `{id:int}` sulla rotta esistente evita il conflitto.
5. API: aggiornare la tabella endpoint e la descrizione di `POST /api/todos` in `api/README.md`; `dotnet test api`.
6. Web, contratto (CLAUDE.md del meta-repo): in `todos.js` aggiungere `isOverdue(todo, today)` pura (`!todo.done && !!todo.dueDate && todo.dueDate < today`, confronto tra stringhe ISO) e una piccola funzione per la data locale del browser `yyyy-MM-dd`, senza usare `toISOString` (che è UTC); `addTodo(title, dueDate)` invia `dueDate` solo se valorizzato. Test in `todos.test.js`: scaduto, oggi, senza data, completato, `addTodo` con e senza data.
7. Web: in `app.js` aggiungere accanto al titolo un `<span>` con la data se presente, con classe `overdue` se `isOverdue`; i completati non vengono evidenziati. Campo `<input type="date" id="due">` facoltativo nel form, svuotato dopo l'invio. Usare `textContent`, mai `innerHTML`. `npm test --prefix web`.
8. Il merge va coordinato: API prima, web dopo. Un todo senza `dueDate` resta valido per entrambi.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API/web).
- `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`, `api/README.md`.
- `web/src/todos.js`, `web/src/app.js`, `web/tests/todos.test.js`.
- `features/_template/*`. Non ci sono altre feature con `decisions.md`. Nessun allegato in `/work/inputs`.
- Risposte del team nel thread (D1–D4), registrate in `decisions.md`.

## Rischio
Livello: verde
- Costi: piccoli, circa 2 file di codice per submodule più test.
- Complessità: bassa; il punto delicato è "oggi", che per scelta del team è UTC nell'API e locale nel browser.
- Sicurezza: nessun dato sensibile; la data è validata dal binding, il web usa `textContent`.
- Accoppiamento: il contratto API/web cambia, ma il campo è facoltativo e additivo, quindi i client esistenti non si rompono. Vicino a mezzanotte UTC l'evidenziazione del web e l'elenco `/overdue` possono dare risultati diversi per un todo: conseguenza voluta di D2.

## Domande aperte
- [x] D1: i completati compaiono tra gli scaduti ed è evidenziata la loro data? — risposta: no, i completati non sono scaduti e non vanno evidenziati
- [x] D2: come si definisce "oggi"? — risposta: data UTC del server per l'API, data locale del browser per il web; un todo che scade oggi non è scaduto
- [x] D3: nome dell'endpoint dei scaduti — risposta: `GET /api/todos/overdue`
- [x] D4: serve un campo data nel form di creazione del web? — risposta: sì, campo data facoltativo
