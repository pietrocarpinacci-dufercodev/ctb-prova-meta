# T-cc62e31c — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md` del meta-repo, dopo il blocco ` ```bash ... ``` ` del clone (in fondo al file), aggiungere una riga vuota, il titolo `## Prova cartella persistente` e una riga di testo (testo proposto, salvo risposta a D1: "Sezione di prova per verificare che la cartella di lavoro del bot resti persistente tra i job.").
2. Verificare con `git diff --stat` che l'unico file cambiato sia `README.md` e che i puntatori di `api` e `web` non si siano spostati.
3. Commit `[T-cc62e31c] Sezione "Prova cartella persistente" nel README`; descrizione PR in `/work/out/meta.md` (nessuna PR sui submodule).
4. Build e test non necessari (solo documentazione): `dotnet test api` e `npm test --prefix web` non vanno rieseguiti; indicarlo nella PR.

## Fonti
- Richiesta del thread (testo nel messaggio del job).
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (comandi, regola sull'agente `backend-dev`).
- `features/T-9dfe1d36/decisions.md` (D2/D6: delega a `backend-dev` negli execute).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: trascurabili, una sola riga di documentazione.
- Complessità: nessuna, nessun codice.
- Sicurezza: nessun impatto; nessun segreto o configurazione coinvolta.
- Accoppiamento: nessuno; submodule e contratto API/web invariati.

## Domande aperte
- [ ] D1: Quale testo deve avere la riga della sezione? Il thread non lo specifica; in mancanza di risposta si usa il testo proposto al passo 1 — fonti: richiesta del thread ("una riga di testo") vs spec.md (contenuto non definito)
- [ ] D2: L'execute deve delegare questa modifica solo documentale all'agente `backend-dev` o può farla direttamente? La regola chiede la delega "per ogni lavoro sul codice" e l'agente è pensato per .NET — fonti: `CLAUDE.md` del meta-repo e `features/T-9dfe1d36/decisions.md` (D2, D6) vs natura della richiesta (solo README)
