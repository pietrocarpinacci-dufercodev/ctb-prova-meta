# T-2f9745c1 — Stato di salute dell'applicazione

## Richiesta
Il team ha chiesto nel thread Teams: "sviluppami una api che restituisce l'heath dell'applicazione e una pagina di FE dove si può vedere lo stato".

## Criteri di accettazione
- WHEN un client invia `GET /api/health`, l'API SHALL rispondere 200 con `status` uguale a `"ok"`, `uptimeSeconds` (intero >= 0) e `version`.
- WHERE un client usa già `status` di `/api/health`, l'API SHALL mantenere invariato il campo `status` (modifica additiva).
- WHEN l'utente apre la pagina web, la pagina SHALL chiamare `/api/health` e mostrare lo stato in testo ("ok") con indicatore visivo.
- IF `/api/health` risponde con errore o non è raggiungibile THEN la pagina SHALL mostrare lo stato "non raggiungibile" senza bloccare l'elenco delle cose da fare.
- WHILE la pagina è aperta, la pagina SHALL aggiornare lo stato ogni 30 secondi (soggetto a D3).
- Il payload di `/api/health` SHALL NOT contenere stringhe di connessione, percorsi di file o altri dati sensibili.
- WHEN si eseguono `dotnet test api` e `npm test --prefix web`, i test SHALL passare, inclusi quelli nuovi su health.
