# T-7f36e180 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. In `README.md`, dopo il blocco `git clone`, aggiungere la sezione `## Prove automatiche` di due righe di testo: il canale di prova riceve messaggi del programma `ctb-e2e`, usati per la verifica end-to-end del bridge.
2. Controllare che la sezione abbia due righe di testo e che `git status` non mostri modifiche in `api/` e `web/`.
3. Nessun test da eseguire: modifica solo documentale.

## Fonti
- Richiesta nel thread Teams.
- `README.md` e `CLAUDE.md` del meta-repo (nessun riferimento esistente a `ctb-e2e`).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una sezione di README.
- Complessità: minima.
- Sicurezza: nessun segreto né permesso toccato.
- Accoppiamento: nessuno, i submodule non cambiano.

## Domande aperte
Nessuna.
