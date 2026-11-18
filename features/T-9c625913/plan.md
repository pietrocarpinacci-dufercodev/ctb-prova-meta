# T-9c625913 — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. Nel meta-repo, sul branch `claude/T-9c625913` (partito da `e2e-base`), aggiungere in fondo a `README.md`, dopo il blocco `git clone`, l'intestazione `## Prova branch di partenza` seguita da una riga di testo (testo proposto in `spec.md`, salvo risposta diversa a D1).
2. Committare solo `README.md` (più `features/T-9c625913/decisions.md` se aggiornato) con messaggio `[T-9c625913] Sezione "Prova branch di partenza" nel README`: niente `git add -A`, niente puntatori dei submodule (il working tree ha già `api` modificato perché è sulla cima di `e2e-base`).
3. Verifica: `git diff e2e-base -- README.md` mostra solo righe aggiunte; `git show --stat HEAD` non elenca `api` né `web`. Nessun build o test da eseguire: la modifica è solo documentale.
4. Scrivere `/work/out/meta.md` con la descrizione della PR del meta-repo; nessuna descrizione per i submodule.

## Fonti
- Richiesta nel thread Teams della feature (testo riportato in `spec.md`).
- `README.md` del meta-repo (struttura attuale: titolo, descrizione submodule, branch base, blocco `git clone`).
- `CLAUDE.md` del meta-repo (struttura e comandi).
- `features/*/decisions.md`: nessuna decisione su README o branch di partenza.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, nessun codice né test toccati.
- Sicurezza: nessun impatto, nessun segreto né configurazione coinvolti.
- Accoppiamento: nessuno; i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? La richiesta non lo specifica; se non arriva risposta si usa il testo proposto in `spec.md` — fonti: richiesta nel thread vs `spec.md`
- [ ] D2: In `api` il branch di partenza è `e2e-base`, la cui cima differisce dal puntatore registrato nel meta-repo, e il runner registra i puntatori dei submodule che si sono spostati anche solo per il merge con il branch di partenza: la PR del meta-repo potrebbe quindi includere uno spostamento del puntatore di `api`. Va bene così o il puntatore va lasciato com'è su `e2e-base`? — fonti: richiesta "Non toccare i submodule" vs regole del runner sulla registrazione dei puntatori
