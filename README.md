# ctb-prova-meta

Meta-repo di prova per claude-teams-bridge (bridge Teams → Claude Code). Contiene due submodule:

- `api/` → [ctb-prova-api](https://github.com/pietrocarpinacci-dufercodev/ctb-prova-api), Web API .NET 10;
- `web/` → [ctb-prova-web](https://github.com/pietrocarpinacci-dufercodev/ctb-prova-web), pagina HTML/JS statica.

Branch base: `develop` in tutti e tre i repo. Le feature richieste dal bot finiscono sui branch `claude/<feature-id>` e in PR in bozza; piani e decisioni stanno in `features/<feature-id>/`.

```bash
git clone --recurse-submodules https://github.com/pietrocarpinacci-dufercodev/ctb-prova-meta.git
```

## Prova cartella persistente

Sezione di prova per verificare che la cartella di lavoro del bot resti persistente tra i job.

followup 1

followup 2

dopo pulisci
