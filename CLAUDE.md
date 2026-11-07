# ctb-prova — meta-repo

Progetto di prova per la verifica end-to-end di claude-teams-bridge: un elenco di cose da fare con API .NET e pagina web statica. Nessun cliente reale.

Le regole operative del bot Teams arrivano dal runner (D45): qui solo le regole del progetto, valide anche per chi lavora in locale.

## Struttura

| Submodule | Percorso | Ruolo | Branch base |
| --- | --- | --- | --- |
| ctb-prova-api | api | Web API .NET 10 (cose da fare in memoria) | develop |
| ctb-prova-web | web | pagina HTML/JS statica che usa l'API | develop |

Il web chiama l'API su `http://localhost:5000` (endpoint `/api/todos` con `title` e `priority`, `/api/todos/{id}` anche con PATCH parziale, `/api/todos/{id}/done`, `/api/health`; vedi `api/README.md`). Un cambio al contratto dell'API va riportato anche in `web/src/todos.js` e nei test di entrambi.

## Comandi

- API: `dotnet test api` (build e test).
- Web: `npm test --prefix web` (runner integrato di Node, nessuna dipendenza da installare).
