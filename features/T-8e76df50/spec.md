# T-8e76df50 — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-8e76df50: "Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule."
Branch di partenza: meta-repo → `e2e-base`, `api` → `e2e-base`, `web` → `develop`.

## Criteri di accettazione
- Il `README.md` del meta-repo SHALL contenere un titolo di sezione `## Prova branch di partenza`, posto in fondo al file dopo il blocco `git clone`.
- La sezione `Prova branch di partenza` SHALL contenere esattamente una riga di testo (testo proposto in D1 del piano).
- WHEN la modifica viene committata, il meta-repo SHALL cambiare solo `README.md` e i file in `features/T-8e76df50/`, senza modificare i submodule `api` e `web` né i loro puntatori.
- Il contenuto esistente del `README.md` SHALL restare invariato.
