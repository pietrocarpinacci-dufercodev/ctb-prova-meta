# e2e-33fcd31c — Piano

## Submodule coinvolti
- Nessuno: nessun submodule viene toccato (richiesta: non toccare `api/` né `web/`).

## Passi
1. Sciogliere D1 (testo della riga) e D2 (posizione) nel thread; aggiornare questo piano e `spec.md`.
2. Aggiungere in fondo a `README.md` (meta-repo) la sezione `## Prova comandi` con una sola riga di testo, come da D1 e D2.
3. Non modificare `api/` né `web/`.
4. Verificare a mano con `git diff`: deve comparire solo `README.md`, nessun puntatore di submodule. Non ci sono test eseguibili pertinenti: `dotnet test api` e `npm test --prefix web` non cambiano per una modifica di sola documentazione. Lo si dichiara nell'esito.
5. Nell'execute: committare solo `README.md` con `git add README.md` (mai `git add -A` né `git commit -a`), messaggio `[e2e-33fcd31c] <cosa>` in italiano. Aggiornare `decisions.md` con le risposte D1 e D2.
6. Scrivere `/work/out/meta.md` con le sezioni richieste dal bot. Non ci sono descrizioni per submodule.

## Fonti
- Richiesta nel messaggio del job (thread Teams): sezione "Prova comandi" con una riga, nessun submodule toccato.
- `README.md` del meta-repo (struttura attuale, nessuna sezione "Prova comandi").
- `CLAUDE.md` del meta-repo (struttura, comandi di test, regole di progetto).
- `features/T-9dfe1d36/decisions.md` e `plan.md` (nessuna decisione in conflitto; usato per il formato).
- `features/_template/*` (struttura dei file della feature).
- `/work/inputs`: cartella vuota, nessun allegato usato.

## Rischio
Livello: verde
- Costi: una riga di markdown nel README, nessun costo di runtime né di build.
- Complessità: minima, nessun codice.
- Sicurezza: nessun segreto, nessun permesso, nessuna dipendenza coinvolta.
- Accoppiamento: nessuno con i submodule; il README è letto solo come documentazione.

## Domande aperte
- [ ] D1: Qual è il testo della riga da scrivere sotto "Prova comandi"? La richiesta non lo indica; proposta: "Riga di prova per la verifica dei comandi del bot." — fonti: richiesta del thread vs richiesta stessa (testo non indicato)
- [ ] D2: Dove collocare la sezione nel README? Proposta: in fondo, dopo il blocco `git clone` — fonti: richiesta del thread vs `README.md` (struttura attuale, nessuna posizione indicata)
