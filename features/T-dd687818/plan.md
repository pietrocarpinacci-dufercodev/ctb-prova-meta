# T-dd687818 — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo, come richiesto nel thread ("Non toccare i submodule").

## Passi
1. In `README.md` (meta-repo), in fondo al file e dopo il blocco di codice di clone, aggiungere una riga vuota, il titolo `## Prova chiusura thread`, una riga vuota e la riga `Thread chiuso e riaperto da ctb-e2e.` (posizione scelta in fondo al file: vedi D1 in `decisions.md`).
2. Se nel `README.md` esiste già una sezione `## Prova chiusura thread`, aggiornare quella invece di aggiungerne un'altra (criterio EARS di `spec.md`).
3. Verificare con `git diff -- README.md` che il diff contenga solo l'aggiunta della sezione e che il testo esistente sia invariato.
4. Verificare con `git status` che non ci siano modifiche nei submodule `api/` e `web/` né in altri file del meta-repo oltre a `README.md` e `features/T-dd687818/`.
5. Aggiornare `features/T-dd687818/decisions.md` con le decisioni prese nell'execute, se ce ne sono di nuove.
6. Scrivere `/work/out/meta.md` con le sezioni richieste dal bot. Nessun submodule toccato, quindi nessun `/work/out/<nome>.md`.

Nessun test automatico da eseguire: il README non ha test; `dotnet test api` e `npm test --prefix web` non sono pertinenti e il loro esito non va riportato come verifica della modifica.

## Fonti
- Richiesta nel thread Teams (testo della richiesta riportato in `spec.md`).
- `README.md` del meta-repo (struttura attuale: titolo, elenco dei submodule, paragrafo sui branch, blocco di clone).
- `CLAUDE.md` del meta-repo (regole di struttura e di progetto; nessuna regola sul README).
- `features/_template/*` per la struttura dei file della feature.
- Regole del bot (D45) per il job di pianificazione e la lingua del progetto.
- Allegati in `/work/inputs`: nessuno disponibile nel job.

## Rischio
Livello: verde
- Costi: una riga di testo più un titolo in un file markdown; nessun costo di build o runtime.
- Complessità: minima, nessuna logica.
- Sicurezza: nessun segreto, nessun cambio di permessi o configurazione; la riga è un testo fisso fornito dal thread.
- Accoppiamento: nessuno; il README non è letto da codice o test.

## Domande aperte
Nessuna.
