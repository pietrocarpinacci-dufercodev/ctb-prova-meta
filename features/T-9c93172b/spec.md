# T-9c93172b — Sezione "Prova cartella persistente" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-9c93172b: "Aggiungi al README del meta-repo una sezione "Prova cartella persistente" con una riga di testo. Non toccare i submodule."

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere un titolo di sezione `## Prova cartella persistente` dopo il contenuto esistente.
- WHEN si legge la sezione `## Prova cartella persistente`, la sezione SHALL contenere esattamente una riga di testo non vuota.
- IF la modifica tocca file diversi da `README.md` e `features/T-9c93172b/*` THEN la feature SHALL essere considerata non conforme.
- WHERE i submodule `api` e `web` sono presenti, i loro puntatori nel meta-repo SHALL restare invariati rispetto a `develop`.
