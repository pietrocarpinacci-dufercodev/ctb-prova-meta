# T-81ccbc6d — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-81ccbc6d (link al thread non disponibile nel job): "Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule."

Branch di partenza indicati dal bot: meta-repo → `e2e-base`, api → `e2e-base`, web → `develop`.

## Criteri di accettazione
- WHEN la feature è completata, il `README.md` del meta-repo SHALL contenere un'intestazione di secondo livello `## Prova branch di partenza`.
- WHEN la feature è completata, la sezione `## Prova branch di partenza` SHALL contenere esattamente una riga di testo non vuota.
- WHEN la feature è completata, il contenuto già presente nel `README.md` SHALL restare invariato.
- WHEN la feature è completata, i puntatori dei submodule `api` e `web` nel meta-repo SHALL restare invariati e nessun file dentro `api/` o `web/` SHALL risultare modificato.
