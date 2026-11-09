# T-54c5d30f — Endpoint versione API

## Richiesta
Il team ha chiesto nel thread Teams: "Aggiungi un endpoint GET /api/health/version che restituisce la versione dell'API."

## Criteri di accettazione
- WHEN un client invia GET /api/health/version, l'API SHALL rispondere 200 con JSON `{ "version": "<versione>" }`.
- WHEN un client invia GET /api/health/version, l'API SHALL restituire la versione dell'assembly dell'API (vedi D1 in plan.md).
- WHEN un client invia GET /api/health, l'API SHALL continuare a rispondere `{ "status": "ok" }` senza modifiche.
- WHERE l'endpoint è documentato, `api/README.md` SHALL elencarlo nella tabella degli endpoint.
