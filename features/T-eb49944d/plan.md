# T-eb49944d — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo il `README.md` del meta-repo. `api` e `web` non si toccano.

## Passi
1. In `README.md`, dopo il blocco `git clone`, aggiungere `## Prove automatiche` con due righe di testo: il canale di prova riceve messaggi del programma `ctb-e2e` (nome tra apici inversi, come da richiesta nel thread); i messaggi sono automatici e non richieste di persone (testo esatto da confermare in D1).
2. Verificare con `git diff` che cambi solo `README.md` e che `git submodule status` non mostri differenze.

## Fonti
- `README.md` e `CLAUDE.md` del meta-repo.
- Richiesta nel thread. Nessun allegato in `/work/inputs` usato.
- Nel repo non c'è altra documentazione di ctb-e2e (cercato con grep).

## Rischio
Livello: verde
- Costi: trascurabili, una modifica di documentazione.
- Complessità: minima.
- Sicurezza: nessun segreto coinvolto.
- Accoppiamento: nessuno, i submodule restano invariati.

## Domande aperte
- [ ] D1: Quale testo esatto vuoi per le due righe? Non so cosa sia ctb-e2e né quale sia il "canale di prova" (nome o link del canale Teams?) — fonti: richiesta vs README (nessuna descrizione nel repo)
