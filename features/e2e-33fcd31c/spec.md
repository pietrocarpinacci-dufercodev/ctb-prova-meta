# e2e-33fcd31c — Sezione "Prova comandi" nel README del meta-repo

## Richiesta
Nel thread Teams il team ha chiesto di aggiungere al README del meta-repo una sezione "Prova comandi" con una riga di testo, senza toccare i submodule. Link al thread: non presente nel messaggio del job.

## Criteri di accettazione
- WHEN il README del meta-repo viene letto, il sistema SHALL contenere una sezione con titolo `## Prova comandi`.
- WHEN la sezione "Prova comandi" è presente, il sistema SHALL contenere sotto il titolo una sola riga di testo, con il testo definito in D1.
- WHERE la sezione "Prova comandi" è presente, il sistema SHALL collocarla nella posizione definita in D2.
- WHILE la feature è in esecuzione, il sistema SHALL lasciare invariato il resto del contenuto di `README.md`.
- WHEN la feature è completata, il sistema SHALL NOT modificare file dentro `api/` né `web/`.
- WHEN il commit della feature viene creato, il sistema SHALL NOT registrare modifiche ai puntatori dei submodule.
- IF la richiesta chiede di modificare un submodule THEN il sistema SHALL NOT eseguire la modifica e SHALL chiedere conferma nel thread.
