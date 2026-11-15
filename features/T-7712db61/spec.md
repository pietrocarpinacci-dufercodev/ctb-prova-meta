# T-7712db61 — Somma dei numeri pari nell'api

## Richiesta
Dal thread Teams della feature T-7712db61 (link al thread non disponibile nel job): "aggiungi nel submodule api una funzione che calcoli la somma dei numeri pari di una lista, con un commento che spieghi cosa fa e un test".

Interpretazione del piano: una funzione C# pura nel submodule `api`, senza nuovi endpoint HTTP e senza cambi al contratto usato da `web` (vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN la funzione riceve una lista di interi, il submodule api SHALL restituire la somma dei soli elementi pari (`n % 2 == 0`).
- WHEN la lista contiene numeri pari negativi, il submodule api SHALL includerli nella somma (es. `[-2, 3, 4]` → `2`).
- WHEN la lista è vuota o non contiene numeri pari, il submodule api SHALL restituire `0`.
- IF la lista è `null` THEN il submodule api SHALL lanciare `ArgumentNullException`.
- IF la somma supera il range di `int` THEN il submodule api SHALL restituire il valore corretto senza overflow (tipo di ritorno `long`, vedi D2 in `plan.md`).
- WHERE la funzione è presente, il codice SHALL avere un commento in inglese (doc comment `///`) che spieghi cosa fa.
- WHERE la funzione è presente, il progetto `api/tests/CtbProva.Api.Tests` SHALL contenere test xUnit che coprono i casi sopra, e `dotnet test api` SHALL passare.
- WHERE la funzione è presente, gli endpoint esistenti (`/api/health`, `/api/todos`, `/api/todos/{id}`, `/api/todos/{id}/done`) SHALL restare invariati.
