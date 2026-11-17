# T-4d5e822a — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md` aggiungere in fondo (dopo il blocco `git clone`) una sezione `## Prove automatiche` di due righe di testo: il canale di prova riceve messaggi del programma `ctb-e2e` (prove automatiche end-to-end), non da persone. Il nome del programma va scritto tra apici inversi, come richiesto nel thread. Aggiungere poi una riga che spieghi che le PR delle prove restano aperte. Il piano assume che diventi la terza riga della sezione (vedi D3).
2. Verificare che `git diff` tocchi solo `README.md` e `features/T-4d5e822a/` e che i puntatori dei submodule non cambino.
3. Scrivere la descrizione della PR in `/work/out/meta.md`.

## Fonti
- `README.md` e `CLAUDE.md` del meta-repo.
- Nessun allegato in `/work/inputs`. Nel repo non c'è altra documentazione su `ctb-e2e`.

## Rischio
Livello: verde
- Costi: trascurabili, poche righe di documentazione.
- Complessità: nessuna.
- Sicurezza: nessun segreto né dato reale coinvolto.
- Accoppiamento: nessuno, i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale canale è "il canale di prova" (nome o identificativo del canale Teams) e va indicato nel README? Nel repo non è definito — fonti: richiesta vs README/CLAUDE.md
- [ ] D2: Le due righe vanno scritte in italiano come il resto del README (assunzione del piano)? — fonti: richiesta vs README
- [ ] D3: La riga sulle PR delle prove che restano aperte si aggiunge alle due già previste, per un totale di tre righe, oppure la sezione deve restare di due righe e il testo va riscritto per farcela stare? — fonti: prima richiesta ("sezione di due righe") vs richiesta successiva ("aggiungi una riga")
- [ ] D4: "Restano aperte" vuol dire che le PR delle prove non vengono mai unite né chiuse, o solo che restano aperte finché qualcuno non le chiude a mano? Il README dice già che le feature finiscono "in PR in bozza" — fonti: richiesta vs README.md
