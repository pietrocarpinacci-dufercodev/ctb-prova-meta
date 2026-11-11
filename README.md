# ctb-prova-meta

Meta-repo di prova per claude-teams-bridge (bridge Teams → Claude Code). Contiene due submodule:

- `api/` → [ctb-prova-api](https://github.com/pietrocarpinacci-dufercodev/ctb-prova-api), Web API .NET 10;
- `web/` → [ctb-prova-web](https://github.com/pietrocarpinacci-dufercodev/ctb-prova-web), pagina HTML/JS statica.

Branch base: `develop` in tutti e tre i repo. Le feature richieste dal bot finiscono sui branch `claude/<feature-id>` e in PR in bozza; piani e decisioni stanno in `features/<feature-id>/`.

```bash
git clone --recurse-submodules https://github.com/pietrocarpinacci-dufercodev/ctb-prova-meta.git
```

Ultimo aggiornamento: 2026-11-11 08:06 UTC
