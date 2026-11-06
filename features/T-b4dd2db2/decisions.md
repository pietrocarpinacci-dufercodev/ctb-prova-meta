# T-b4dd2db2 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-06 | Si crea un nuovo endpoint `DELETE /api/todos/{id}` (oggi non esiste) (D1) | thread | risposta a D1 |
| 2026-11-06 | Il FE ottiene un pulsante "Elimina" per elemento; al click si apre un popup di conferma prima di chiamare DELETE (D3) | thread | operazione distruttiva; contratto API da riportare nel web |
| 2026-11-06 | DELETE su id inesistente risponde 404 (D2) | thread | coerente con GET e `/done` |
| 2026-11-06 | Il corpo del 200 contiene solo l'ID, non l'oggetto rimosso (sostituisce la richiesta iniziale) | thread | indicazione successiva nel thread |
| 2026-11-06 | Formato del corpo: `{"id": 3}`; l'id mantiene il suo tipo (oggi intero; se fosse un GUID, si riporta il GUID) (D4) | thread | risposta a D4 |
