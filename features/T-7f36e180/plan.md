# T-7f36e180 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. In `README.md`, dopo il blocco `git clone`, aggiungere la sezione `## Prove automatiche` con:
   - una riga che dice che il canale di prova riceve messaggi del programma `` `ctb-e2e` `` (nome tra apici inversi), usati per la verifica end-to-end del bridge;
   - una riga che dice che le PR delle prove restano aperte.
   Il numero totale di righe dipende dalla risposta a D1.
2. Controllare che la sezione abbia il numero di righe deciso con D1 e che `git status` non mostri modifiche in `api/` e `web/`.
3. Nessun test da eseguire: modifica solo documentale.

## Fonti
- Richieste nel thread Teams (sezione di due righe, nome tra apici inversi, riga sulle PR aperte).
- `README.md` e `CLAUDE.md` del meta-repo (nessun riferimento esistente a `ctb-e2e`).
- `features/*/decisions.md`: nessuna decisione sulla chiusura delle PR delle prove.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una sezione di README.
- Complessità: minima.
- Sicurezza: nessun segreto né permesso toccato.
- Accoppiamento: nessuno, i submodule non cambiano.

## Domande aperte
- [ ] D1: La riga sulle PR aperte porta la sezione a tre righe, o la sezione deve restare di due righe (fondendo il testo sul canale `ctb-e2e` in una sola riga)? — fonti: prima richiesta del thread ("sezione di due righe", spec.md) vs nuova richiesta del thread ("aggiungi una riga")
