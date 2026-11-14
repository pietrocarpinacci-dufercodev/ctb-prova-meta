# T-cc62e31c — Sezione "Prova cartella persistente" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-cc62e31c: "Aggiungi al README del meta-repo una sezione "Prova cartella persistente" con una riga di testo. Non toccare i submodule." (link al thread non disponibile nel job).

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere un titolo di secondo livello `## Prova cartella persistente`.
- WHEN si legge la sezione `## Prova cartella persistente`, la sezione SHALL contenere esattamente una riga di testo non vuota sotto il titolo.
- WHERE la sezione è aggiunta, il README SHALL mantenere invariato il contenuto esistente (titolo, elenco submodule, branch base, comando di clone).
- IF la modifica tocca file dentro `api/` o `web/` o sposta i loro puntatori THEN la PR SHALL essere considerata non conforme.
