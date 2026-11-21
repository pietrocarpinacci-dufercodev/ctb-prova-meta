# T-144b7b69 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-21 | Nell'API la priorità resta una stringa del JSON validata su un elenco chiuso, senza enum né converter | bot (piano) | Codice minimo; il valore resta nella forma del contratto |
| 2026-11-21 | Nel web la priorità si mostra con un'etichetta di testo oltre al colore | bot (piano) | Accessibilità: il colore da solo non basta |
| 2026-11-21 | Priorità di default `media`, preselezionata anche nel menu del web (D1) | thread | Proposta del piano accettata |
| 2026-11-21 | Endpoint dedicato `PUT /api/todos/{id}/priority` con body `{ "priority": "..." }` (D2) | thread | Proposta del piano accettata |
| 2026-11-21 | Valori `bassa`/`media`/`alta` nel JSON; identificatori in inglese (D3) | thread | Valori richiesti nel thread; proposta accettata |
| 2026-11-21 | UI del web in italiano, messaggi di validazione nuovi dell'API in inglese (D4) | thread | Proposta del piano accettata |
| 2026-11-21 | Filtro `GET /api/todos?priority=` con valore non ammesso → 400 (D5) | thread | Proposta del piano accettata; convenzione "input non valido → 400" |
| 2026-11-21 | Ordine dell'elenco invariato in API e web (D6) | thread | Proposta del piano accettata |
| 2026-11-21 | Nel web niente UI per cambiare la priorità o filtrare; `setPriority` solo in `todos.js` (D7) | thread | Proposta del piano accettata |
| 2026-11-21 | Lavoro su `api` e `web` delegato a `backend-dev` (D8) | thread | Proposta del piano accettata; il `CLAUDE.md` lo chiede per ogni lavoro sul codice |
| 2026-11-21 | `priority` è facoltativo in `POST /api/todos`: il contratto resta compatibile con i client esistenti | bot (piano) | Nessuna rottura per chi crea todo senza priorità |
