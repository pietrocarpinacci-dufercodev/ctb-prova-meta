# T-c0f1fe3d — Piano

## Submodule coinvolti
- Nessuno: `api/README.md` viene solo letto. L'unico file prodotto è `docs/revisione-readme.md` nel meta-repo.

## Passi
1. Nell'execute, delegare all'agente `backend-dev` (tool Agent, `subagent_type: backend-dev`) la lettura di `api/README.md` e la revisione: punti poco chiari e informazioni mancanti, al massimo 5, confrontando il README con `api/src/CtbProva.Api/Program.cs` e con i contratti citati nel CLAUDE.md.
2. Scrivere l'esito in `docs/revisione-readme.md` (crearlo; se esiste, aggiungere in coda). Formato: elenco numerato, max 5 punti, ognuno con tipo (poco chiaro / mancante) e suggerimento.
3. Aggiornare `decisions.md` se emergono decisioni nuove; scrivere `/work/out/meta.md` con le sezioni richieste dalle regole del bot (nessun test da eseguire: solo documentazione).
4. Nella risposta finale scrivere esattamente il nome degli agenti usati: `backend-dev`.
5. Commit locale `[T-c0f1fe3d] Revisione README api`.

## Fonti
- `.claude/agents/backend-dev.md` (unico agente presente)
- `.gitmodules` e `CLAUDE.md` (ordine dei submodule, regole di delega)
- `api/README.md` (oggetto della revisione, da leggere nell'execute)
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: una sola invocazione di agente, un file markdown.
- Complessità: minima.
- Sicurezza: nessun segreto coinvolto, nessuna modifica al codice o cancellazione.
- Accoppiamento: nessuno; `docs/` non è `docs/superpowers/`.

## Domande aperte
- [ ] D1: `backend-dev` è descritto come agente per lavoro sul codice .NET, non per revisioni di documentazione. Va bene usarlo comunque per questa revisione (unico agente disponibile), o si preferisce un altro agente? — fonti: richiesta nel thread vs `.claude/agents/backend-dev.md` e CLAUDE.md
