# Revisione di api/README.md

Agente: backend-dev

1. Informazione mancante: il POST `/api/todos` risponde 201 con `Location` e la cosa creata, ma il README non indica codice di successo né forma della risposta; il 400 è un ValidationProblem (`errors.title`), non descritto.
2. Poco chiaro: "400 se il titolo è vuoto" non dice che vale anche per titolo mancante o solo spazi, e che il titolo valido viene ripulito dagli spazi ai bordi (trim).
3. Informazione mancante: `POST /api/todos/{id}/done` non dichiara il 404 per id inesistente né cosa restituisce (la cosa aggiornata con `done: true`).
4. Informazione mancante: non è descritto il modello di una cosa da fare (`id` intero, `title`, `done`), né che `{id}` deve essere intero (altrimenti la rotta non corrisponde e risponde 404) né il formato di `/api/health` (`{ "status": "ok" }`).
5. Poco chiaro: i comandi `dotnet build`/`dotnet test` non dicono da quale cartella lanciarli (nel meta-repo si usa `dotnet test api`); mancano porta di default (il web usa `http://localhost:5000`) e il fatto che CORS è aperto a qualsiasi origine.
