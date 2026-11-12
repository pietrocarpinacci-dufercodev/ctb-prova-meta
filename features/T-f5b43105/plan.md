# T-f5b43105 — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. In `README.md` del meta-repo, dopo il blocco `git clone` (in fondo al file), aggiungere:
   ```markdown

   ## Prova branch di partenza

   <riga di testo, vedi D1>
   ```
2. Verificare con `git diff --stat` che cambi solo `README.md` e che i puntatori di `api` e `web` siano invariati (`git diff --submodule` vuoto).
3. Commit nel meta-repo `[T-f5b43105] Sezione "Prova branch di partenza" nel README`, aggiungendo solo `README.md` e i file di `features/T-f5b43105/`.
4. Nessun test da eseguire: modifica solo documentale (build e test di `api` e `web` non interessati).

Nota: la delega a `backend-dev` prevista dal `CLAUDE.md` riguarda il lavoro sul codice; qui c'è solo una riga di README, quindi l'execute può farla direttamente (da scrivere nell'esito).

## Fonti
- Richiesta nel thread Teams della feature.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sull'agente `backend-dev`).
- `features/T-9dfe1d36/decisions.md`: nessun conflitto.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, un solo file.
- Sicurezza: nessun impatto, nessun segreto né codice eseguibile.
- Accoppiamento: nessuno, i submodule non vengono toccati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? Proposta se non indicato: "Sezione aggiunta per verificare che il bot parta dal branch indicato nel job (meta-repo da `e2e-base`)." — fonti: richiesta del thread ("una riga di testo", contenuto non specificato)
