# T-c4e76874 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-10 | La rotta `/api/todos/export` resta separata da `/api/todos/{id:int}` grazie al vincolo `:int` già presente | bot (piano) | evita collisioni senza cambiare le rotte esistenti |
| 2026-11-10 | Escaping CSV con una funzione locale, nessuna dipendenza nuova | bot (piano) | poche righe di logica, basta la libreria standard |
