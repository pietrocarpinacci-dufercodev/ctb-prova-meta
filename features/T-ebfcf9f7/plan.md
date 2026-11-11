# T-ebfcf9f7 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md`, dopo la riga del `git clone` (o dopo il paragrafo "Branch base"), aggiungere una riga di testo fisso con la data del giorno di esecuzione, per esempio `Ultimo aggiornamento: 2026-11-11` (formato da confermare in D1).
2. Verificare con `git diff` che cambi solo quella riga.
3. Scrivere `/work/out/meta.md` (nessun submodule toccato, nessun test applicabile).

## Fonti
- `README.md` e `CLAUDE.md` del meta-repo.
- Richiesta del thread (nessun allegato in `/work/inputs`).

## Rischio
Livello: verde
- Costi: una riga di documentazione.
- Complessità: nulla, nessun codice.
- Sicurezza: nessun impatto.
- Accoppiamento: nessuno, nessun contratto API/web toccato.

## Domande aperte
- [ ] D1: "la data di oggi" va scritta come testo fisso (data del giorno dell'execute) e in quale formato e posizione (proposta: `Ultimo aggiornamento: AAAA-MM-GG` in fondo al README)? — fonti: richiesta del thread vs README.md
