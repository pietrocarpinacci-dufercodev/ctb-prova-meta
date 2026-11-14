# T-29eb5c84 — Elenco dei file .md del repository con un sotto-agente Explore

## Richiesta
Dal thread Teams della feature T-29eb5c84: "usa un sotto-agente Explore per elencare i file .md del repository, senza modificare nulla".

## Criteri di accettazione
- WHEN parte l'execute della feature, il bot SHALL delegare la ricerca dei file `.md` a un sotto-agente di tipo `Explore` (tool Agent, `subagent_type: Explore`).
- WHEN il sotto-agente termina, il bot SHALL riportare nell'esito del job l'elenco dei percorsi relativi di tutti i file `.md` del meta-repo e dei submodule `api` e `web`, raggruppati per repository.
- WHILE l'execute è in corso, il bot SHALL lasciare invariati il codice e i file dei submodule `api` e `web`.
- IF un file `.md` corrisponde ai pattern delle credenziali (`.env*`, `*.pem`, `*.key`, `id_rsa*`, `secrets*`, `credentials*`) THEN il bot SHALL elencarne solo il percorso, senza leggerne il contenuto.
- WHERE si trovano file `.md` sotto `.git/` o `node_modules/`, il bot SHALL escluderli dall'elenco.
