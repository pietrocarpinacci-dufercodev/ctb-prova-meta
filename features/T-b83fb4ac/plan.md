# T-b83fb4ac — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. In `README.md`, dopo il blocco `git clone`, aggiungere `## Prove automatiche` con queste righe:
   - una riga che dice che il canale di prova riceve messaggi del programma `` `ctb-e2e` ``, con il nome tra apici inversi;
   - una riga che dice che le PR delle prove restano aperte;
   - solo se la risposta a D1 è "tre righe", una terza riga che precisa che i messaggi sono automatici, non di persone.
2. Verificare che `git status` mostri solo `README.md` modificato e nessun puntatore di submodule spostato.
3. Scrivere la descrizione PR in `/work/out/meta.md`.

Proposta per D1: tenere due righe, sostituendo con la riga sulle PR la seconda riga del piano precedente ("messaggi automatici, non di persone"). Quella riga era un'aggiunta del bot e la prima riga già dice che i messaggi arrivano da un programma.

## Fonti
- `README.md` e `CLAUDE.md` del meta-repo.
- `features/T-b83fb4ac/spec.md` e `decisions.md` (versione precedente del piano).
- Richieste del thread: sezione di due righe, nome del programma tra apici inversi, riga sulle PR che restano aperte.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una sola modifica di documentazione.
- Complessità: nessuna.
- Sicurezza: nessun segreto coinvolto.
- Accoppiamento: nessuno, submodule intatti.

## Domande aperte
- [ ] D1: La sezione resta di due righe (la riga sulle PR sostituisce quella sui "messaggi automatici, non di persone", proposta del piano) o diventa di tre righe? — fonti: richiesta iniziale del thread ("sezione di due righe") vs richiesta del thread "aggiungi alla sezione una riga sul fatto che le PR delle prove restano aperte"
