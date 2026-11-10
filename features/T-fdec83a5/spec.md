# T-fdec83a5 — API health del backend

## Richiesta
Il team ha chiesto nel thread Teams di sviluppare un'API che restituisce l'health dell'applicazione backend, senza sviluppo sul frontend. Alla domanda D1 ha risposto di aggiungere la versione di .NET in uso. In fase di execute va indicato quale subagent è stato usato.

## Criteri di accettazione
- WHEN un client invia `GET /api/health`, il backend SHALL rispondere `200 OK` con corpo JSON contenente `status` = `"ok"`.
- WHEN un client invia `GET /api/health`, il backend SHALL includere nel corpo il campo `dotnetVersion` uguale a `Environment.Version.ToString()` del processo in esecuzione.
- WHEN un client invia `GET /api/health`, il backend SHALL rispondere senza autenticazione e senza dipendere dallo stato di `TodoStore`.
- IF il campo `status` è letto da `web/src/todos.js`, THEN il backend SHALL mantenerlo invariato (il campo extra è solo additivo).
- WHEN l'execute termina, l'esito SHALL indicare il subagent usato (atteso: `backend-dev`) o che si è lavorato senza agente.
- Il frontend (`web`) SHALL restare invariato.
