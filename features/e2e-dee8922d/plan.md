# e2e-dee8922d — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo. `api/` e `web/` non vanno toccati.

## Passi
1. Attendere la risposta alla domanda D1 (testo della riga). Se il team approva con `[approva comunque]`, usare il testo proposto in D1.
2. In `README.md` (meta-repo), in fondo al file, aggiungere una riga vuota e la sezione `## Prova comandi` con una sola riga di testo in italiano.
3. Non modificare `api/`, `web/`, `.gitmodules`, `CLAUDE.md` né altri file del meta-repo.
4. Verificare a mano: il diff tocca solo `README.md`, il titolo è di secondo livello come le altre sezioni, la riga è in italiano e il markdown si renderizza. Nessun comando di build o test è pertinente: non c'è codice.
5. Nell'execute: committare solo `README.md` sul branch `claude/e2e-dee8922d` (messaggio `[e2e-dee8922d] Aggiunta sezione Prova comandi al README`), senza `git add -A`; scrivere `/work/out/meta.md`. Nessun file `/work/out/<submodule>.md` perché nessun submodule è toccato.

## Fonti
- Richiesta del thread Teams (riportata nel messaggio del job di pianificazione): sezione "Prova comandi" con una riga di testo, submodule non toccati.
- `README.md` del meta-repo (struttura attuale: titolo, descrizione, blocco `git clone`).
- `CLAUDE.md` del meta-repo (regole di progetto: nessun comando da eseguire per una modifica solo documentale).
- `.gitmodules` (submodule `api` e `web`, branch `develop`).
- `features/T-9dfe1d36/*` e `features/_template/*` (formato di spec, plan e decisions).
- Nessun allegato in `/work/inputs`: la cartella è vuota.

## Rischio
Livello: verde
- Costi: un file markdown, nessun costo di runtime né di build.
- Complessità: minima; una sezione con una riga.
- Sicurezza: nessuna superficie esposta; il testo non deve contenere segreti o stringhe di connessione.
- Accoppiamento: nessuno con i submodule; il README non è letto da altri file del repo.

## Domande aperte
- [ ] D1: Qual è il testo della riga da mettere sotto "Prova comandi"? La richiesta non lo specifica. Proposta di default: "Questa sezione serve a verificare il flusso end-to-end del bot Teams." — fonti: richiesta del thread vs assenza di testo definito nella spec
