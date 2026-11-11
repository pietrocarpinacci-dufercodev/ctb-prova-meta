# T-eb49944d — Sezione "Prove automatiche" nel README

## Richiesta
Il team ha chiesto nel thread Teams del bot: aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe, che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Non toccare i submodule.

## Criteri di accettazione
- Il `README.md` del meta-repo SHALL contenere una sezione con titolo "Prove automatiche".
- Il corpo della sezione SHALL essere di due righe e SHALL dire che il canale di prova riceve messaggi del programma `ctb-e2e`, con il nome scritto tra apici inversi.
- WHEN il job è eseguito, il sistema SHALL lasciare invariati i submodule `api` e `web`.
