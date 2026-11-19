# T-b11744ea — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo; `api/` e `web/` non si toccano (richiesta del thread).

## Passi
1. Risolvere D1 (testo della riga) nel thread prima dell'execute.
2. Aggiungere in fondo a `README.md`, dopo il blocco ```bash di clone, una riga vuota e la sezione `## Prova chiusura thread` con una sola riga di testo (contenuto da D1).
3. Non modificare il resto di `README.md` (titolo, elenco dei submodule, link, blocco di clone).
4. Verificare con `git diff --stat` che l'unico file cambiato sia `README.md` e che la sezione compaia una sola volta (Grep su `Prova chiusura thread`).
5. Nell'execute scrivere `/work/out/meta.md` con le sezioni richieste dal bot. Nessun `/work/out/<submodule>.md`, perché nessun submodule viene toccato.

## Fonti
- Richiesta nel messaggio del job (thread Teams, link non fornito nel job).
- `README.md` del meta-repo (lingua, struttura, posizione della nuova sezione).
- `.gitmodules` (submodule `api` e `web`, da non toccare).
- `CLAUDE.md` del meta-repo (solo le regole di struttura; nessun comando specifico per il README).
- `features/T-9dfe1d36/plan.md` e `decisions.md`, `features/_template/*` (formato dei file della feature).
- Allegati in `/work/inputs`: nessuno trovato.

## Rischio
Livello: verde
- Costi: una sezione di una riga in un file markdown, nessun costo di runtime.
- Complessità: minima; nessun codice, nessun test eseguibile.
- Sicurezza: nessuna superficie; il testo della riga non deve contenere segreti o dati sensibili.
- Accoppiamento: nessuno con i submodule; non risultano altri file che leggano il README.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga sotto "Prova chiusura thread"? Il messaggio del job chiede solo "una riga di testo" senza indicarla, quindi non la inventerò. — fonti: richiesta nel thread (riga di testo non specificata) vs README (nessun testo previsto per la sezione)
