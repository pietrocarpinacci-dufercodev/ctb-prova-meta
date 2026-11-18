# T-d554ce65 — Sezione "Prove automatiche" nel README

## Richiesta
Il team ha chiesto nel thread Teams: aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe che spieghi che il canale di prova riceve messaggi del programma ctb-e2e, scrivendo il nome del programma tra apici inversi. Non toccare i submodule.

## Criteri di accettazione
- WHEN il job di esecuzione termina, il `README.md` del meta-repo SHALL contenere una sezione con titolo `## Prove automatiche`.
- WHEN la sezione `## Prove automatiche` è presente, il sistema SHALL contenere due righe di testo che spiegano che il canale di prova riceve messaggi del programma `ctb-e2e`.
- WHEN il testo della sezione viene scritto, il nome del programma SHALL comparire tra apici inversi (`` `ctb-e2e` ``).
- WHILE il job modifica il `README.md`, il sistema SHALL lasciare invariato il testo esistente del file.
- IF il job modifica file nei submodule `api` o `web` THEN il job SHALL considerarsi non conforme.
