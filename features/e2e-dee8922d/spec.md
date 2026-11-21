# e2e-dee8922d — Sezione "Prova comandi" nel README del meta-repo

## Richiesta
Nel thread Teams (link non fornito nel messaggio del job) il team ha chiesto di aggiungere al README del meta-repo una sezione "Prova comandi" con una riga di testo, senza toccare i submodule.

## Criteri di accettazione
- WHEN il README del meta-repo viene letto, il sistema SHALL contenere una sezione di titolo `## Prova comandi`.
- WHEN la sezione `## Prova comandi` è presente, il sistema SHALL contenere in essa esattamente una riga di testo in italiano (il testo è in attesa di conferma, vedi D1 in `plan.md`).
- WHEN la modifica è applicata, il sistema SHALL mantenere invariato il contenuto esistente del README.
- WHEN la modifica è applicata, il sistema SHALL NOT modificare i file dei submodule `api` e `web`, né i loro puntatori registrati nel meta-repo.
- WHEN la modifica è applicata, il sistema SHALL modificare solo il file `README.md` del meta-repo.
