# T-3a7239fb — Piano

## Stato
Piano provvisorio: la richiesta «riprova» non contiene un'attività concreta. Nessuna modifica al codice è pianificata finché non viene chiarita.

## Submodule coinvolti
- Nessuno per ora (api e web non cambiano).

## Passi
1. Il team indica nel thread cosa va ripetuto (la richiesta originale, o il messaggio/job che è fallito).
2. Con la richiesta chiara, aggiornare `spec.md` con i criteri di accettazione e riscrivere questo piano, indicando per ogni submodule (api, web) i file toccati. Se cambia il contratto dell'API, riportarlo in `web/src/todos.js` e nei test di entrambi.
3. Dopo l'approvazione, la fase di esecuzione implementa sui branch `claude/T-3a7239fb` dei submodule, verificando con `dotnet test api` e `npm test --prefix web`.

## Domande aperte
- Cosa si intende per «riprova»? Quale richiesta o quale job precedente va rieseguito?
- Se si riferisce a un'altra feature (per esempio T-2d17ef32 o T-a251369e, che hanno già un piano), va indicato l'id corretto.
