# T-744349be — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-18 | La sezione va in fondo a `README.md`, come titolo di secondo livello (`##`), dopo il blocco `git clone` | Claude (piano) | Non altera il testo esistente; la richiesta non indica una posizione |
| 2026-11-18 | Nessun submodule coinvolto e nessun uso del subagent `backend-dev` nell'execute | Claude (piano) | La richiesta vieta di toccare i submodule; la modifica è solo documentazione, non codice |
| 2026-11-18 | Testo della riga: «Sezione di prova per verificare la cartella persistente del bot.» (proposta di D1) | Claude (execute) | Piano approvato con `[approva comunque]` e D1 aperta: si applica il testo proposto in spec.md |
| 2026-11-18 | Modifica fatta senza il subagent `backend-dev` | Claude (execute) | Solo documentazione nel meta-repo, nessun codice (come da piano) |
| 2026-11-18 | Aggiunta alla sezione la riga «followup 1», dopo la riga esistente e separata da una riga vuota | Thread (followup 1) | Richiesta del thread; supera il criterio "esattamente una riga" della spec, aggiornata di conseguenza. La riga vuota evita che Markdown la unisca al paragrafo precedente |
