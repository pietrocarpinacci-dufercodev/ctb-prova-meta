# T-d554ce65 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo il `README.md` del meta-repo; `api` e `web` non si toccano.

## Passi
1. Aggiungere in fondo a `README.md` la sezione `## Prove automatiche` con due righe di testo: il canale di prova riceve messaggi del programma ctb-e2e (testo esatto da confermare in D1).
2. Verificare che il resto del README sia invariato (`git diff`).
3. Scrivere `/work/out/meta.md` come descrizione della PR (nessun submodule toccato, nessun test da eseguire).

## Fonti
- Richiesta nel thread Teams.
- `README.md` e `CLAUDE.md` del meta-repo.
- `features/T-9dfe1d36/decisions.md` (nessun conflitto).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: minimi, un solo file di documentazione.
- Complessità: nessuna.
- Sicurezza: nessun segreto coinvolto.
- Accoppiamento: nessuno, submodule intatti.

## Domande aperte
- [ ] D1: Quale canale di prova (Teams? quale?) e che cosa significa ctb-e2e (nome del programma, dove vive)? Nel repo non compaiono né "ctb-e2e" né "canale di prova": servono il testo esatto o il significato per non inventare — fonti: richiesta vs README e CLAUDE.md
