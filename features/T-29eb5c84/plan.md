# T-29eb5c84 — Piano

## Submodule coinvolti
Nessuno: la richiesta è di sola lettura. `api` e `web` vengono solo letti dal sotto-agente, non modificati.

## Passi
1. Avviare un sotto-agente `Explore` (tool Agent, `subagent_type: Explore`, ampiezza "medium") con il compito di elencare i file `*.md` in `/work/repo`, compresi i submodule `api` e `web`, escludendo `.git/` e `node_modules/`, senza leggerne il contenuto e senza modificare nulla.
2. Verificare l'elenco ricevuto con un controllo indipendente (Glob `**/*.md`) per non riportare risultati mancanti o inventati.
3. Riportare nell'esito del job l'elenco raggruppato per repository (meta-repo, `api`, `web`) con i percorsi relativi.
4. Aggiornare `features/T-29eb5c84/decisions.md` con le decisioni prese e scrivere `/work/out/meta.md` come richiesto dalle regole dell'execute (vedi D2 sulle eventuali modifiche al meta-repo).

## Fonti
- Richiesta nel thread Teams della feature T-29eb5c84.
- `CLAUDE.md` del meta-repo (struttura submodule, regola sul subagent `backend-dev`).
- `features/T-9dfe1d36/decisions.md` (D2, D6: execute delegato a `backend-dev` per il lavoro sul codice).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: un solo sotto-agente in lettura, pochi file.
- Complessità: minima, nessun codice da scrivere.
- Sicurezza: sola lettura; i file con credenziali non vanno letti (sono esclusi anche se avessero estensione `.md`).
- Accoppiamento: nessuno, nessun contratto tra submodule toccato.

## Domande aperte
- [ ] D1: Dove va consegnato l'elenco: solo nell'esito del job nel thread, oppure anche in un file versionato (es. `features/T-29eb5c84/md-files.md`)? — fonti: richiesta "senza modificare nulla" vs regole execute (commit e descrizioni PR)
- [ ] D2: "Senza modificare nulla" vale anche per il meta-repo? Le regole dell'execute chiedono di aggiornare `decisions.md` e scrivere le descrizioni PR, che producono un commit nel meta-repo — fonti: richiesta nel thread vs regole del bot sui job execute
- [ ] D3: Il `CLAUDE.md` e `features/T-9dfe1d36/decisions.md` (D2, D6) chiedono di delegare l'execute a `backend-dev`; la richiesta chiede un sotto-agente `Explore`. Confermate che per questo compito di sola lettura si usa `Explore` e non `backend-dev`? — fonti: richiesta nel thread vs `CLAUDE.md` del meta-repo e decisioni T-9dfe1d36
- [ ] D4: L'elenco deve includere anche i file del meta-repo fuori dal codice (`features/`, `.claude/agents/`, template) o solo la documentazione di progetto (README, CLAUDE.md)? — fonti: richiesta "file .md del repository" (ambigua sul perimetro)
