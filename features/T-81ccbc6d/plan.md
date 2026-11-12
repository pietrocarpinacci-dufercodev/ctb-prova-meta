# T-81ccbc6d — Piano

## Submodule coinvolti
Nessuno: la richiesta tocca solo `README.md` del meta-repo ("Non toccare i submodule").

## Passi
1. In `README.md` del meta-repo, dopo il blocco `git clone` finale, aggiungere una riga vuota, l'intestazione `## Prova branch di partenza` e una sola riga di testo (testo proposto: "Sezione aggiunta per verificare il flusso del bot a partire da un branch di partenza diverso da `develop`.", salvo risposta diversa a D1).
2. Verificare con `git diff --stat` che cambi solo `README.md` e con `git submodule status` / `git diff` che i puntatori di `api` e `web` siano invariati.
3. Committare in locale `[T-81ccbc6d] README: sezione Prova branch di partenza` e scrivere `/work/out/meta.md`. Nessuna descrizione PR per i submodule (non toccati).
4. Build e test: non necessari (solo documentazione); facoltativi `dotnet test api` e `npm test --prefix web` per conferma che nulla sia cambiato.

Nota: il CLAUDE.md chiede di delegare a `backend-dev` il lavoro sul codice; qui si modifica solo documentazione, quindi l'execute può procedere senza agente e lo segnala nell'esito.

## Fonti
- Richiesta nel thread (messaggio del job plan).
- `README.md` del meta-repo (struttura attuale: introduzione, submodule, branch base, comando di clone).
- `CLAUDE.md` del meta-repo (regola su `backend-dev`).
- `features/T-9dfe1d36/decisions.md` (nessuna decisione in conflitto).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, un solo file Markdown.
- Sicurezza: nessun impatto, nessun codice né configurazione.
- Accoppiamento: nessuno, i submodule e il contratto API restano invariati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione "Prova branch di partenza"? Se non indicato si usa il testo proposto al passo 1 — fonti: richiesta nel thread ("una riga di testo", contenuto non specificato) vs spec.md
