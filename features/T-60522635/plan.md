# T-60522635 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. Aggiungere in fondo a `README.md` del meta-repo, dopo il blocco `git clone`, la sezione:
   ```markdown
   ## Prova branch di partenza

   Questa sezione verifica che il bot parta dal branch indicato nel job (qui `e2e-base`) e non da `develop`.
   ```
2. Verificare con `git diff` che cambi solo `README.md` (più i file in `features/T-60522635/`) e che i puntatori di `api` e `web` non siano toccati.
3. Committare `[T-60522635] Sezione "Prova branch di partenza" nel README` aggiungendo solo `README.md` e i file della feature (niente `git add -A`).

Build e test dei submodule non servono: nessun codice cambia.

## Fonti
- Richiesta nel thread Teams della feature.
- `README.md` e `CLAUDE.md` del meta-repo.
- `features/*/decisions.md`: nessuna decisione su README o branch di partenza.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: nulli, una riga di documentazione.
- Complessità: minima, nessun codice.
- Sicurezza: nessun impatto, nessun segreto né configurazione.
- Accoppiamento: nessuno, i submodule non vengono toccati.

## Domande aperte
Nessuna. Il testo della riga non è specificato nella richiesta: si usa quello proposto al passo 1 (vedi `decisions.md`), modificabile nel thread prima dell'approvazione.
