# T-29eb5c84 — Piano

## Submodule coinvolti
Nessuno: la richiesta è di sola lettura. `api` e `web` vengono solo letti dal sotto-agente, non modificati.

## Passi
1. Avviare un sotto-agente con il tool Agent e `subagent_type: Explore` con il compito di contare i file `*.md` in `/work/repo`, compresi i submodule `api` e `web`, escludendo `.git/` e `node_modules/`, senza leggerne il contenuto e senza modificare nulla. Il prompt chiede di restituire il totale (e, per controllo interno, il dettaglio per repository).
2. Non fare ricerche dirette (Glob, Grep, shell) sui file `.md`: il conteggio è solo quello del sotto-agente, come chiesto nel followup.
3. Riportare nell'esito del job solo il numero totale.
4. Aggiornare `features/T-29eb5c84/decisions.md` e scrivere `/work/out/meta.md` come richiesto dalle regole dell'execute (vedi D2).

## Fonti
- Richiesta e followup nel thread Teams della feature T-29eb5c84.
- `CLAUDE.md` del meta-repo (struttura submodule, regola sul subagent `backend-dev`).
- `features/T-9dfe1d36/decisions.md` (D2, D6: execute delegato a `backend-dev` per il lavoro sul codice).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: un solo sotto-agente in lettura, pochi file.
- Complessità: minima, nessun codice da scrivere.
- Sicurezza: sola lettura; i file con nome da credenziali vengono contati ma non letti.
- Accoppiamento: nessuno, nessun contratto tra submodule toccato.

## Domande aperte
- [x] D1: Dove va consegnato il risultato: nell'esito del job o in un file versionato? — risposta: solo il numero, nell'esito del job (followup nel thread)
- [ ] D2: "Senza modificare nulla" vale anche per il meta-repo? Le regole dell'execute chiedono di aggiornare `decisions.md` e scrivere le descrizioni PR, che producono un commit nel meta-repo — fonti: richiesta nel thread vs regole del bot sui job execute
- [x] D3: Si usa `Explore` invece di `backend-dev` indicato dal `CLAUDE.md`? — risposta: sì, tool Agent con `subagent_type: Explore`, e il bot non cerca direttamente (followup nel thread)
- [ ] D4: Il conteggio include anche i `.md` interni al meta-repo (`features/`, `.claude/agents/`, template) o solo la documentazione di progetto (README, CLAUDE.md)? — fonti: followup "file .md del repository" (ambiguo sul perimetro)
