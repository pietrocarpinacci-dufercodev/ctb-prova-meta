# T-4ab341d4 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-11 | La `priority` in creazione è opzionale (default da D1), il campo è additivo nel JSON | bot (piano) | Compatibilità con il web attuale e con client che non la inviano |
| 2026-11-11 | Valori non ammessi per `priority` (creazione, modifica, filtro) rispondono 400 | bot (piano) | Convenzione di validazione ai confini dell'API (`backend-dev.md`) |
| 2026-11-11 | Nel web la priorità è mostrata come testo oltre che a colori | bot (piano) | Accessibilità: il colore non deve essere l'unica indicazione |
| 2026-11-11 | Priorità di default `media`, preselezionata anche nel menu del web (D1) | thread | Proposta del piano accettata |
| 2026-11-11 | Ordine: il web mette prima i non fatti, poi alta → media → bassa, poi id; l'API resta ordinata per id (D2) | thread | Proposta del piano accettata |
| 2026-11-11 | Valori ammessi solo `bassa`/`media`/`alta` minuscoli, altrimenti 400 (D3) | thread | Proposta del piano accettata |
| 2026-11-11 | Endpoint di modifica `PUT /api/todos/{id}/priority` con corpo `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata |
| 2026-11-11 | Il filtro `?priority=` accetta un solo valore (D5) | thread | Proposta del piano accettata |
| 2026-11-11 | Il web non modifica la priorità né filtra l'elenco in questa feature (D6) | thread | Proposta del piano accettata |
| 2026-11-11 | Tutto il lavoro sul codice, api e web, delegato a `backend-dev`; nessun agente Frontend nuovo (D7) | thread / bot (piano) | Il thread chiede la scelta più semplice: un solo agente, regola del `CLAUDE.md` rispettata, D4 di T-9dfe1d36 invariata |
| 2026-11-11 | Colori: bassa verde, media ambra, alta rosso | thread | La proposta del piano diceva "grigio/verde" per bassa: scelto verde |
