# Revisione di api/README.md

Eseguita dall'agente `backend-dev` (T-c0f1fe3d). Confronto basato sulla lettura di `api/README.md` e `api/src/CtbProva.Api/Program.cs`; l'API non è stata avviata.

1. **Mancante** — Forma della risposta e codici di successo. Il README non indica il modello `{ id, title, done }`. Il codice risponde 201 con header `Location` alla creazione e 200 sugli altri endpoint. *Suggerimento:* aggiungere il modello `Todo` e i codici 200/201.
2. **Poco chiaro** — `POST /api/todos/{id}/done` non dice cosa succede se l'id non esiste (il codice risponde 404, come gli altri endpoint). *Suggerimento:* scrivere "404 se non esiste" e dire che l'operazione è idempotente.
3. **Poco chiaro** — "400 se il titolo è vuoto" è impreciso. Il codice usa `Results.ValidationProblem` (`application/problem+json`, `errors.title`); un titolo di soli spazi conta come vuoto e il titolo salvato viene tagliato con `Trim()`. *Suggerimento:* scrivere "titolo assente, vuoto o di soli spazi" e indicare il formato dell'errore.
4. **Mancante** — CORS e porta. Il CORS è aperto a qualsiasi origine, header e metodo, ma non è documentato. Il web usa `http://localhost:5000`, mentre `dotnet run` non fissa la porta. *Suggerimento:* documentare il CORS e l'avvio con `--urls http://localhost:5000`.
5. **Mancante** — Vincoli e casi limite. `{id}` deve essere intero (altrimenti 404), non c'è lunghezza massima del titolo, non esistono PUT/DELETE, `GET /api/todos` ordina per id. Il README dice `dotnet test`, il meta-repo usa `dotnet test api` dalla radice. *Suggerimento:* aggiungere una nota su id numerici, ordinamento e directory dei comandi.
