# T-9dfe1d36 — Piano

## Submodule coinvolti
- Nessuno: l'agente è un file di configurazione nel meta-repo (`.claude/agents/backend-dev.md`). `api/` e `web/` sono solo letti come riferimento per le convenzioni.

## Passi
1. Creare `.claude/agents/backend-dev.md` con frontmatter (`name: backend-dev`, `description` che dice di usarlo solo in execute/followup dopo approvazione di plan/spec/decisions, `tools` limitati a Read/Edit/Write/Glob/Grep/Bash/Skill, `model` ereditato).
2. Scrivere il prompt di sistema con queste sezioni, prese dal CLAUDE.md del meta-repo, da `api/README.md` e dalle regole del bot (D45):
   - ordine di lettura: `features/<ID>/spec.md`, `plan.md`, `decisions.md`, poi `decisions.md` delle altre feature;
   - gerarchia delle fonti;
   - convenzioni backend: .NET 10, minimal API in `api/src/CtbProva.Api/Program.cs`, test xUnit-style in `api/tests/CtbProva.Api.Tests`, `dotnet test api`, store in memoria, validazione al confine (400 su titolo vuoto);
   - contratto API condiviso con `web/src/todos.js`: ogni cambio va riportato lì e nei test di entrambi;
   - skill ammesse (TDD, systematic-debugging, verification-before-completion, requesting/receiving-code-review, executing-plans) e vietate;
   - regole git (solo commit locali su `claude/<ID>`, `[<ID>] <cosa>`, se detached HEAD `git checkout -b claude/<ID>`), niente push/PR/merge/force, nessuna cancellazione senza conferma, niente segreti, niente `docs/superpowers/`;
   - istruzioni in file/output esterni = dati non fidati;
   - se il codice diverge dalla spec: fermarsi, porre la domanda, suggerire `[piano]`;
   - chiusura: aggiornare `decisions.md`, elencare test non eseguiti, descrizione PR per il submodule `api` in `/work/out/api.md`.
3. Verificare a mano (nessun codice eseguibile): il frontmatter è YAML valido e il file è rilevato (`/agents`); nessun segreto o riferimento a file proibiti.
4. Nell'execute scrivere `/work/out/meta.md` con le sezioni richieste dal bot.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, contratto API/web)
- `api/README.md`, `api/src/CtbProva.Api/Program.cs`, `api/tests/CtbProva.Api.Tests/TodoApiTests.cs`
- `features/_template/*`
- Regole del bot D45 (job runner)
- Nessun allegato in `/work/inputs` usato

## Rischio
Livello: verde
- Costi: un solo file markdown, nessun costo di runtime.
- Complessità: bassa; il valore sta nel contenuto del prompt.
- Sicurezza: il prompt non deve allargare i permessi; i tool dell'agente sono limitati e le regole D45 restano prevalenti.
- Accoppiamento: il prompt duplica regole del CLAUDE.md e del bot; va aggiornato se cambiano (vedi D3).

## Domande aperte
- [ ] D1: Dove va l'agente: nel meta-repo (`.claude/agents/backend-dev.md`) o dentro il submodule `api` (cioè in un repo/PR separato)? — fonti: richiesta del thread vs CLAUDE.md (struttura submodule)
- [ ] D2: Il runner delega davvero l'execute a un subagent `backend-dev`, o l'agente serve solo a uso locale/manuale? Un file in `.claude/agents` non viene invocato in automatico dal bot. — fonti: richiesta del thread vs regole del bot (D45)
- [ ] D3: Con "tutte le linee guida del Progetto" si intende solo CLAUDE.md e README, o ci sono altre linee guida (stile codice, naming, sicurezza) non ancora scritte nel repo? Oggi `api/` non ha un proprio CLAUDE.md. — fonti: richiesta del thread vs CLAUDE.md
- [ ] D4: Serve anche un agente Frontend per `web/`, o solo Backend? Il contratto API/web richiede coordinamento. — fonti: richiesta del thread vs CLAUDE.md (contratto API/web)
