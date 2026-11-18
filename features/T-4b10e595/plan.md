# T-4b10e595 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. Aggiungere in fondo a `README.md` del meta-repo la sezione `## Prova cartella persistente` seguita da una riga di testo (testo proposto: «Sezione di prova per verificare la cartella persistente del runner.», salvo risposta diversa a D1).
2. Verificare con `git diff` che sia cambiato solo `README.md` e che i puntatori di `api` e `web` siano invariati.
3. Committare nel meta-repo con il messaggio `[T-4b10e595] Sezione "Prova cartella persistente" nel README`.
4. Nessuna build o test da eseguire: la modifica è solo documentazione.

## Fonti
- Richiesta nel thread Teams della feature T-4b10e595.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura e comandi del progetto).
- `features/*/decisions.md`: nessuna decisione riguarda il README.
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, nessun codice.
- Sicurezza: nessun impatto, nessun segreto o configurazione coinvolta.
- Accoppiamento: nessuno, i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? Proposta: «Sezione di prova per verificare la cartella persistente del runner.» — fonti: richiesta nel thread ("una riga di testo", contenuto non specificato) vs spec.md
