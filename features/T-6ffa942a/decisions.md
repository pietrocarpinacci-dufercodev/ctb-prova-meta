# T-6ffa942a — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-19 | `priority` come stringa validata contro i tre valori ammessi, senza enum né converter JSON | bot (piano) | Il contratto è già una stringa; meno codice, validazione con `ValidationProblem` come per il titolo |
| 2026-11-19 | Priorità di default `media`, anche come valore preimpostato del menu web (D1) | thread | Proposta del piano accettata |
| 2026-11-19 | Ordine: nel web da fare prima, poi alta → bassa, poi id; l'API resta ordinata per id (D2) | thread | Proposta del piano accettata |
| 2026-11-19 | Valori nel JSON `bassa`/`media`/`alta` letterali (D3) | bot (piano), su delega del thread | Domanda senza proposta: scelta la più semplice, senza mappatura valori/etichette; sono valori del contratto chiesti dal team, mentre identificatori e commenti restano in inglese |
| 2026-11-19 | Endpoint dedicato `POST /api/todos/{id}/priority` con corpo `{ "priority": "..." }` (D4) | thread | Proposta del piano accettata, coerente con `POST .../done` |
| 2026-11-19 | Filtro con valore non valido → 400; confronto sensibile alle maiuscole, solo minuscolo (D5) | thread | Proposta del piano accettata |
| 2026-11-19 | Il web mostra la priorità e la fa scegliere in creazione, senza modifica né filtro (D6) | thread | Proposta del piano accettata |
| 2026-11-19 | Nell'execute `api` è delegato a `backend-dev`, `web` si fa senza agente (D7) | bot (piano), su delega del thread | Domanda senza proposta: scelta la più semplice, coerente con T-9dfe1d36 D4 |
| 2026-11-19 | In `POST /api/todos/{id}/priority` la priorità si valida prima dell'id: un id inesistente con priorità mancante o non valida risponde 400, con priorità valida 404 | bot (execute) | La spec non fissa l'ordine; validare prima l'input evita una lettura dello store inutile |
| 2026-11-19 | Messaggio di errore sulla priorità in inglese (`Priority must be one of: bassa, media, alta.`), quello già esistente sul titolo resta in italiano | bot (execute) | Regola delle lingue: testi nel codice in inglese, testo esistente invariato |
| 2026-11-19 | Nel web il badge della priorità è escluso dallo stile barrato dei todo fatti (`li.done span:first-child`) | bot (execute) | La priorità resta leggibile anche sui todo completati |
| 2026-11-19 | Il test sul rifiuto di una priorità non valida in creazione chiesto nel thread copre il tipo JSON sbagliato (`priority: 5` → 400, todo non salvato); i valori stringa non validi erano già coperti da `Invalid_priority_on_create_is_rejected_and_not_stored` e, nel web, da `addTodo rifiuta una priorità non valida` | bot (followup) | Evitare un test duplicato e coprire il caso rimasto scoperto |
