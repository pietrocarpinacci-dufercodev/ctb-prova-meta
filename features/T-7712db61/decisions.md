# T-7712db61 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-15 | Proposta: funzione statica `EvenNumbers.Sum(IEnumerable<int>)` in un file nuovo del progetto api, senza endpoint (in attesa di D1) | bot (piano) | La richiesta parla di "una funzione"; nessun cambio al contratto con il web |
| 2026-11-15 | Proposta: ritorno `long`, `null` → `ArgumentNullException`, pari negativi inclusi (in attesa di D2, D3) | bot (piano) | Correttezza sui casi limite a costo zero |
| 2026-11-15 | Piano approvato con `[approva comunque]`: D1-D3 restano aperte, implementata la spec approvata (solo funzione, nessun endpoint; ritorno `long`; input `IEnumerable<int>`) | thread | Regola del bot: con domande aperte si segue la spec approvata |
| 2026-11-15 | Implementazione delegata al subagent `backend-dev`, verifica finale fatta dal runner | bot (execute) | Regola di delega del `CLAUDE.md` del meta-repo |
