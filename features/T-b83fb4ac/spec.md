# T-b83fb4ac — Sezione "Prove automatiche" nel README

## Richiesta
Aggiungere al README del meta-repo una sezione "Prove automatiche" di due righe che spieghi che il canale di prova riceve messaggi del programma ctb-e2e. Il nome del programma va scritto tra apici inversi. Non toccare i submodule. (Richiesta dal thread Teams del bot, con la precisazione sugli apici inversi.)

## Criteri di accettazione
- WHEN il `README.md` del meta-repo viene letto, il sistema SHALL mostrare una sezione con titolo `## Prove automatiche`.
- WHEN la sezione viene letta, il sistema SHALL mostrare un corpo di esattamente due righe di testo.
- WHEN la sezione viene letta, il sistema SHALL indicare che il canale di prova riceve messaggi del programma `` `ctb-e2e` `` (nome tra apici inversi).
- IF la modifica è completa THEN i submodule `api` e `web` SHALL restare invariati (nessun puntatore spostato).
