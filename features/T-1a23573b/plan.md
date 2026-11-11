# T-1a23573b — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. Ottenere la data corrente dal sistema (`date +%F`), non da testo fisso.
2. Aggiungere a `README.md` una riga con la data, nel formato e nella posizione decisi in D1 e D2.
3. Verificare con `git diff` che cambi solo quella riga.
4. Scrivere `/work/out/meta.md` (nessun test da eseguire: modifica solo documentale).

## Fonti
- Richiesta nel thread Teams.
- `README.md` e `CLAUDE.md` del meta-repo.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga.
- Complessità: nessuna.
- Sicurezza: nessun impatto.
- Accoppiamento: nessuno, i submodule non sono toccati.

## Domande aperte
- [ ] D1: quale formato per la data (es. `2026-11-11` ISO o `11/11/2026`)? — fonti: richiesta vs README (nessun formato definito)
- [ ] D2: dove va la riga (in fondo al README, sotto il titolo) e con quale testo (es. `Ultimo aggiornamento: <data>`)? — fonti: richiesta vs README
- [ ] D3: «data di oggi» è quella dell'esecuzione del job o quella della richiesta? Se la riga deve restare aggiornata va rigenerata a ogni modifica? — fonti: richiesta vs spec
