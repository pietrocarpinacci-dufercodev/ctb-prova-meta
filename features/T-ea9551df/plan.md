# T-ea9551df — Piano

## Submodule coinvolti
- nessuno: la modifica riguarda solo `README.md` del meta-repo; `api` e `web` non vanno toccati.

## Passi
1. Sul branch `claude/T-ea9551df` del meta-repo (partito da `e2e-base`), aggiungere in fondo a `README.md`, dopo il blocco `git clone`, una riga vuota, il titolo `## Prova branch di partenza` e la riga di testo indicata in `spec.md`.
2. Verificare con `git diff` che cambino solo quelle righe di `README.md` e che i puntatori di `api` e `web` non siano nell'area di stage (lo stato `M api` iniziale dipende dal submodule sulla cima di `e2e-base`, non va committato).
3. Committare solo `README.md` e `features/T-ea9551df/decisions.md` con messaggio `[T-ea9551df] Sezione "Prova branch di partenza" nel README`.
4. Scrivere `/work/out/meta.md` con la descrizione della PR; nessun file per i submodule. Nessun test da eseguire (solo documentazione).

## Fonti
- Richiesta del thread Teams della feature T-ea9551df.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura e comandi).
- `features/*/decisions.md`: nessuna decisione in conflitto.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una sezione di due righe in un file di documentazione.
- Complessità: minima, nessun codice e nessun test coinvolti.
- Sicurezza: nessun impatto, nessun segreto né configurazione.
- Accoppiamento: nessuno, i submodule e il contratto tra API e web restano invariati.

## Domande aperte
Nessuna: il testo della riga, non indicato nella richiesta, è proposto in `spec.md` e si approva con il piano.
