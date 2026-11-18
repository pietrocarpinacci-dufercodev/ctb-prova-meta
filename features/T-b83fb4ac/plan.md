# T-b83fb4ac — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. In `README.md`, dopo il blocco `git clone`, aggiungere `## Prove automatiche` con due righe. La prima dice che il canale di prova riceve messaggi del programma `` `ctb-e2e` ``, con il nome tra apici inversi come richiesto. La seconda precisa che sono messaggi automatici, non di persone.
2. Verificare che `git status` mostri solo `README.md` modificato e nessun puntatore di submodule spostato.
3. Scrivere la descrizione PR in `/work/out/meta.md`.

## Fonti
- `README.md` e `CLAUDE.md` del meta-repo.
- `features/T-b83fb4ac/spec.md` e `decisions.md` (versione precedente del piano).
- Richiesta del thread: nome del programma tra apici inversi.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una sola modifica di documentazione.
- Complessità: nessuna.
- Sicurezza: nessun segreto coinvolto.
- Accoppiamento: nessuno, submodule intatti.

## Domande aperte
Nessuna.
