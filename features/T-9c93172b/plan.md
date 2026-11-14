# T-9c93172b — Piano

## Submodule coinvolti
- nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md` del meta-repo, dopo il blocco `git clone` finale, aggiungi una riga vuota, il titolo `## Prova cartella persistente` e una riga di testo (testo proposto, salvo risposta diversa a D1: "Sezione di prova per verificare che la cartella di lavoro del bot resti persistente tra i job.").
2. Verifica con `git diff --stat` che cambino solo `README.md` e i file in `features/T-9c93172b/`, e con `git submodule status` che i puntatori di `api` e `web` siano quelli di `develop`.
3. Committa nel meta-repo con `[T-9c93172b] Sezione "Prova cartella persistente" nel README` (solo `README.md` e `features/T-9c93172b/`).
4. Nessun test da eseguire: modifica solo di documentazione (`dotnet test api` e `npm test --prefix web` non sono interessati).

## Fonti
- Richiesta nel thread Teams della feature.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura e comandi).
- `features/T-9dfe1d36/decisions.md` (nessun conflitto).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: nulla, nessun codice.
- Sicurezza: nessun impatto, nessun segreto né configurazione.
- Accoppiamento: nessuno, i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? Se non indicato, uso quello proposto al passo 1 — fonti: richiesta nel thread ("una riga di testo", contenuto non specificato) vs criteri di accettazione in `spec.md`
