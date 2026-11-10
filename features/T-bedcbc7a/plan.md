# T-bedcbc7a — Piano

## Submodule coinvolti
- api: nuovi endpoint `PUT /api/todos/{id}`, `DELETE /api/todos/{id}`, filtro `?done=` su `GET /api/todos`; test; aggiornamento di `README.md`.
- web: solo se D3 risponde che la pagina deve usare i nuovi endpoint (`src/todos.js` e test).

## Passi
1. Leggere codice e test esistenti di `api` (struttura in `src/CtbProva.Api`) e riusare lo store in memoria e le convenzioni già presenti.
2. Una volta risolte le domande aperte, scrivere prima i test (TDD) per i criteri di `spec.md`.
3. Implementare PUT, DELETE e filtro `done` nell'API, senza toccare il contratto degli endpoint esistenti.
4. Aggiornare `api/README.md` con i nuovi endpoint.
5. Se richiesto da D3, riportare il cambio in `web/src/todos.js` e nei test.
6. Eseguire `dotnet test api` e `npm test --prefix web`.
7. Qualsiasi funzione oltre il CRUD (autenticazione, persistenza, ecc.) entra solo dopo la risposta alle domande e con un nuovo piano.

## Fonti
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sul contratto API/web).
- `api/README.md` (endpoint attuali).
- `features/T-9dfe1d36/` e `features/_template/` (formato dei file della feature).
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: giallo
- Costi: basso per il CRUD; senza perimetro la richiesta può crescere senza limite.
- Complessità: bassa per il perimetro proposto; alta se «fare tutto» va preso alla lettera.
- Sicurezza: DELETE rimuove dati (in memoria, progetto di prova); se si aggiungessero autenticazione o persistenza il rischio salirebbe.
- Accoppiamento: un cambio al contratto va riportato in `web/src/todos.js` e nei test di entrambi i submodule.

## Domande aperte
- [ ] D1: Cosa significa «fare tutto»? Quali funzioni servono oltre al CRUD sulle cose da fare (es. utenti, tag, scadenze, ricerca, persistenza su database, autenticazione)? — fonti: richiesta del thread vs `api/README.md` (API di prova in memoria)
- [ ] D2: «Complessissima» è un requisito o solo un modo di dire? Il progetto è una prova e `CLAUDE.md` descrive una API minimale: va bene partire dal solo completamento del CRUD? — fonti: richiesta del thread vs `CLAUDE.md`
- [ ] D3: La pagina web deve usare i nuovi endpoint (modifica e cancellazione dalla UI) o si cambia solo l'API? — fonti: `CLAUDE.md` (contratto API/web) vs richiesta del thread
- [ ] D4: La cancellazione (DELETE) è ammessa? Elimina dati dall'elenco in memoria. — fonti: regole del bot (conferma per le cancellazioni) vs richiesta del thread
