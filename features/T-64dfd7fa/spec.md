# T-64dfd7fa — Sezione "Prova chiusura thread" nel README del meta-repo

## Richiesta
Nel thread Teams (link non disponibile nel contesto del job) il team chiede di aggiungere al README del meta-repo una sezione "Prova chiusura thread" con la sola riga "Thread chiuso e riaperto da ctb-e2e.". Non toccare i submodule e non aggiungere altro.

## Criteri di accettazione
- WHEN il README del meta-repo (`README.md`) viene letto, il sistema SHALL contenere una sezione con titolo "Prova chiusura thread" (heading `## Prova chiusura thread`) posta in fondo al file.
- WHEN la sezione "Prova chiusura thread" è presente, il sistema SHALL contenere come unico contenuto la riga "Thread chiuso e riaperto da ctb-e2e.".
- WHEN la modifica è completata, il sistema SHALL avere come unico file modificato `README.md` nel meta-repo, senza altri file.
- IF la modifica tocca i submodule `api` o `web` (puntatori o contenuto) THEN il sistema SHALL considerare il job non conforme.
- WHILE il README esiste, il sistema SHALL mantenere invariato il contenuto esistente (titolo, descrizione e blocco di clone).
