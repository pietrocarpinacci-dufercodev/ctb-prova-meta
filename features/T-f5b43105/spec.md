# T-f5b43105 — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-f5b43105: "Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule."

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere un'intestazione `## Prova branch di partenza`.
- WHEN si legge la sezione `## Prova branch di partenza`, la sezione SHALL contenere esattamente una riga di testo non vuota sotto l'intestazione.
- WHERE la sezione è presente, il README SHALL mantenere invariato il contenuto esistente (titolo, elenco submodule, branch base, comando di clone).
- IF la modifica tocca file diversi da `README.md` del meta-repo (esclusi i file in `features/T-f5b43105/`) THEN la PR SHALL essere considerata non conforme.
- IF un puntatore di submodule (`api`, `web`) cambia THEN la PR SHALL essere considerata non conforme.
