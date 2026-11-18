# T-744349be — Piano

## Submodule coinvolti
- nessuno: la modifica riguarda solo `README.md` del meta-repo; `api` e `web` non si toccano.

## Passi
1. Aggiungere in fondo a `README.md` del meta-repo, dopo il blocco `git clone`, una riga vuota, il titolo `## Prova cartella persistente` e una sola riga di testo (testo di D1).
2. Verificare con `git diff` che cambi solo `README.md` e che i puntatori di `api` e `web` non siano cambiati.
3. Commit nel meta-repo: `[T-744349be] Sezione "Prova cartella persistente" nel README`. Nessun test da eseguire (solo documentazione); non serve il subagent `backend-dev` perché non c'è codice.

## Fonti
- Richiesta del thread Teams della feature.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura submodule, regola sul subagent `backend-dev`).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: nulla, nessun codice né test coinvolti.
- Sicurezza: nessun impatto; nessun segreto o configurazione toccata.
- Accoppiamento: nessuno, i submodule e il contratto API restano invariati.

## Domande aperte
- [ ] D1: Quale testo va nella riga della sezione? Proposta: «Sezione di prova per verificare la cartella persistente del bot.» — fonti: richiesta del thread ("una riga di testo", contenuto non specificato) vs spec.md
