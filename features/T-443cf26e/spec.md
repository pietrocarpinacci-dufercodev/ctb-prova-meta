# T-443cf26e — Sezione "Prove automatiche" nel README

## Richiesta
Aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Non toccare i submodule. (Thread Teams della feature T-443cf26e.)

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere una sezione con titolo "Prove automatiche".
- Il testo della sezione SHALL dire che il canale di prova riceve messaggi del programma `ctb-e2e`, con il nome scritto tra apici inversi.
- La sezione SHALL contenere anche una riga che dice che le PR delle prove restano aperte (la sezione diventa di tre righe, salvo diversa risposta a D2 del piano).
- IF la feature è completata THEN il diff SHALL toccare solo `README.md` (e `features/T-443cf26e/`), senza modifiche ai submodule `api` e `web`.
