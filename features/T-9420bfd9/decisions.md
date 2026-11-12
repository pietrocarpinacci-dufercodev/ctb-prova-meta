# T-9420bfd9 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-12 | La priorità nel web si vede come testo colorato, non solo come colore | bot (piano) | Accessibilità: l'informazione non dipende solo dal colore |
| 2026-11-12 | In creazione `priority` è facoltativa | bot (piano) | Compatibilità con i client che oggi mandano solo `title` |
| 2026-11-12 | Default `media`, sia nell'API sia come opzione preselezionata nel web (D1) | thread | Proposta del piano accettata |
| 2026-11-12 | L'API resta ordinata per id; il web ordina per stato, poi priorità da alta a bassa, poi id (D2) | thread | Proposta del piano accettata; l'API non cambia ordine e il web mette in evidenza le cose urgenti |
| 2026-11-12 | La priorità nel JSON è una stringa minuscola `bassa`/`media`/`alta`, con confronto esatto (D3) | thread | Proposta del piano accettata; valori come nella richiesta, senza enum né converter |
| 2026-11-12 | Endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }`, che risponde 200, 400 o 404 (D4) | thread | Proposta del piano accettata |
| 2026-11-12 | Un filtro `?priority=` non valido risponde 400 (D5) | thread | Proposta del piano accettata; coerente con la validazione ai confini |
| 2026-11-12 | Filtro e modifica della priorità dal web fuori ambito (D6) | thread | Proposta del piano accettata; la richiesta per il web cita solo visualizzazione e creazione |
| 2026-11-12 | Execute: `backend-dev` per `api`, web senza agente (D7) | thread | Proposta del piano accettata; non c'è un agente Frontend (T-9dfe1d36 D4) |
| 2026-11-12 | Colori: bassa verde, media ambra, alta rosso, contrasto almeno 4.5:1 (D8) | thread | Proposta del piano accettata |
| 2026-11-12 | Nel web i valori di priorità sconosciuti vanno in fondo e non hanno colore | bot (piano) | Scelta più semplice per un caso non coperto dalle domande, come chiesto nel thread |
