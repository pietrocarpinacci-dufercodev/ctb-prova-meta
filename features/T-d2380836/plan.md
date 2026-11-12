# T-d2380836 — Piano

## Submodule coinvolti
- nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. Sul branch `claude/T-d2380836` del meta-repo, aggiungere in fondo a `README.md` la sezione `## Prova branch di partenza` con una sola riga di testo (testo proposto: «Questa sezione verifica che le feature partano dal branch di partenza indicato nel job (qui `e2e-base`).», salvo risposta diversa a D1).
2. Committare solo `README.md` con messaggio `[T-d2380836] Sezione "Prova branch di partenza" nel README` (`git add README.md`, niente `git add -A`: il puntatore di `api` risulta modificato nel working tree e non va committato).
3. Verifica: `git diff --stat` del commit mostra solo `README.md`; `git diff e2e-base...HEAD -- api web` vuoto. Nessun build/test necessario (solo documentazione).
4. Scrivere `/work/out/meta.md` con la descrizione della PR; nessun file per i submodule.

## Fonti
- Richiesta nel thread Teams della feature.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura e comandi).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: nulla, nessun codice.
- Sicurezza: nessun impatto.
- Accoppiamento: nessuno; i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? Il piano propone «Questa sezione verifica che le feature partano dal branch di partenza indicato nel job (qui `e2e-base`).» — fonti: richiesta del thread («una riga di testo», contenuto non specificato) vs plan.md passo 1
