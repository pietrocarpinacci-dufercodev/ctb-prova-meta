# T-3bdadff6 — Piano

## Submodule coinvolti
- Nessuno: il README del submodule `api` (primo in `.gitmodules`) viene solo letto, non modificato. L'unico file prodotto è `docs/revisione-readme.md` nel meta-repo.

## Passi
1. Nell'execute, delegare con il tool Agent (`subagent_type: backend-dev`) la revisione di `api/README.md`: l'unico agente in `.claude/agents` è `backend-dev`, quindi se ne usa uno solo (la regola "due diversi" scatta solo con due o più agenti).
2. Chiedere all'agente al massimo 5 punti, divisi tra "punti poco chiari" e "informazioni mancanti", in sola lettura (nessuna modifica a `api/`). Confrontare il README con `api/src/CtbProva.Api/Program.cs` solo per verificare i punti.
3. Scrivere l'esito in `docs/revisione-readme.md` (creare `docs/` se manca; mai in `docs/superpowers/`): titolo, agente usato, elenco numerato di max 5 punti.
4. Nella risposta finale scrivere esattamente il nome dell'agente usato: `backend-dev`.
5. Descrizione PR del meta-repo in `/work/out/meta.md`. Nessun test da eseguire (nessun codice cambia).

## Fonti
- `.claude/agents/backend-dev.md` (unico agente presente), `CLAUDE.md` del meta-repo (regola di delega), `.gitmodules` (primo submodule: `api`), `features/T-9dfe1d36/decisions.md` (D1: agente solo `backend-dev`; nessun agente Frontend).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: una sola delega, un file di documentazione.
- Complessità: minima.
- Sicurezza: sola lettura del README; nessun segreto coinvolto.
- Accoppiamento: nessun cambio a codice o contratti tra submodule.

## Domande aperte
- [ ] D1: `backend-dev` è definito per lavoro sul codice .NET e non per revisioni di documentazione; va bene usarlo comunque per la revisione del README, dato che è l'unico agente disponibile? — fonti: richiesta del thread vs `.claude/agents/backend-dev.md`
- [ ] D2: Con un solo agente disponibile si usa solo `backend-dev` (la richiesta chiede due agenti solo se ce ne sono due o più): confermi, o vuoi che si definisca un secondo agente (es. documentazione) in un'altra feature? — fonti: richiesta del thread vs `features/T-9dfe1d36/decisions.md`
