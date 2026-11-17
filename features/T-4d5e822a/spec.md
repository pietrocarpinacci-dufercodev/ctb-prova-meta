# T-4d5e822a — Sezione "Prove automatiche" nel README

## Richiesta
Il team ha chiesto nel thread Teams di aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe, che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Non vanno toccati i submodule.

## Criteri di accettazione
- WHEN si apre `README.md` del meta-repo, il README SHALL contenere una sezione con titolo "Prove automatiche".
- Il testo della sezione SHALL essere di due righe e SHALL indicare che il canale di prova riceve messaggi del programma ctb-e2e.
- IF il lavoro viene eseguito, THEN i submodule `api` e `web` SHALL restare invariati (nessun commit, puntatori non spostati).
- Il resto del README SHALL restare invariato.
