# T-29eb5c84 — Conteggio dei file .md del repository con un sotto-agente Explore

## Richiesta
Dal thread Teams della feature T-29eb5c84: "usa un sotto-agente Explore per elencare i file .md del repository, senza modificare nulla". Followup: "usa lo strumento Agent con subagent_type Explore per contare i file .md del repository e riportami solo il numero. Non fare tu la ricerca direttamente."

## Criteri di accettazione
- WHEN parte l'execute della feature, il bot SHALL delegare il conteggio dei file `.md` a un sotto-agente avviato con il tool Agent e `subagent_type: Explore`.
- WHILE l'execute è in corso, il bot SHALL astenersi dal cercare direttamente i file `.md` (nessun Glob, Grep o comando shell di ricerca fatto dal bot).
- WHEN il sotto-agente termina, il bot SHALL riportare nell'esito del job solo il numero totale dei file `.md`, senza elenco dei percorsi.
- WHILE l'execute è in corso, il bot SHALL lasciare invariati il codice e i file dei submodule `api` e `web`.
- IF un file `.md` corrisponde ai pattern delle credenziali (`.env*`, `*.pem`, `*.key`, `id_rsa*`, `secrets*`, `credentials*`) THEN il sotto-agente SHALL contarlo senza leggerne il contenuto.
- WHERE si trovano file `.md` sotto `.git/` o `node_modules/`, il sotto-agente SHALL escluderli dal conteggio.
