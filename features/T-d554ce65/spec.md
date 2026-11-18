# T-d554ce65 — Sezione "Prove automatiche" nel README

## Richiesta
Il team ha chiesto nel thread Teams: aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Non toccare i submodule.

## Criteri di accettazione
- Il `README.md` del meta-repo SHALL contenere una sezione con titolo "Prove automatiche".
- La sezione SHALL essere lunga due righe di testo e SHALL spiegare che il canale di prova riceve messaggi del programma ctb-e2e.
- Il testo esistente del `README.md` SHALL restare invariato.
- IF il job modifica file fuori dal meta-repo THEN il job SHALL considerarsi non conforme: i submodule `api` e `web` non vanno toccati.
