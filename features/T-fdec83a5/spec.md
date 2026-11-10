# T-fdec83a5 — API health del backend

## Richiesta
Il team ha chiesto nel thread Teams di sviluppare un'API che restituisce l'health dell'applicazione backend. Nessuno sviluppo sul frontend. In fase di execute va indicato quale subagent è stato usato.

## Criteri di accettazione
- WHEN un client invia `GET /api/health`, il backend SHALL rispondere `200 OK` con corpo JSON contenente `status` = `"ok"`.
- WHEN un client invia `GET /api/health`, il backend SHALL rispondere senza autenticazione e senza dipendere dallo stato di `TodoStore`.
- WHERE il campo aggiuntivo scelto in D1 è presente (es. `timestamp` UTC), il backend SHALL includerlo nel corpo JSON (da confermare, vedi D1).
- IF il processo è avviato, THEN il backend SHALL mantenere il contratto esistente `{ "status": "ok" }` compatibile con `web/src/todos.js`.
- WHEN l'execute termina, l'esito SHALL indicare il subagent usato (atteso: `backend-dev`) o che si è lavorato senza agente.
- Il frontend (`web`) SHALL restare invariato.
