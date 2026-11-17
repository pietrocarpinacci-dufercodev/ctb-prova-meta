# T-4d5e822a — Sezione "Prove automatiche" nel README

## Richiesta
Il team ha chiesto nel thread Teams di aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe, che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Non vanno toccati i submodule. Richieste successive nel thread: scrivere il nome del programma tra apici inversi e aggiungere una riga sul fatto che le PR delle prove restano aperte.

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere una sezione con titolo "Prove automatiche".
- Il testo della sezione SHALL indicare che il canale di prova riceve messaggi del programma `ctb-e2e`, con il nome del programma tra apici inversi.
- La sezione SHALL contenere una riga che indica che le PR delle prove restano aperte.
- La sezione SHALL avere tre righe di testo: le due della prima richiesta più quella sulle PR (da confermare, vedi D3 in `plan.md`).
- IF il lavoro viene eseguito, THEN i submodule `api` e `web` SHALL restare invariati (nessun commit, puntatori non spostati).
- Il resto del README SHALL restare invariato.
