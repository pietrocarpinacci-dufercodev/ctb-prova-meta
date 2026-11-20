# T-a355ab56 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-20 | Intestazione CSV `titolo,priorita` e valori `bassa`/`media`/`alta` mantenuti in italiano come negli allegati, anche se il codice è in inglese | bot (piano) | Sono dati e formato forniti dagli allegati, non identificatori del codice |
| 2026-11-20 | CSV generato con `StringBuilder` ed escaping RFC 4180, nessuna libreria nuova | bot (piano) | Formato semplice; evita dipendenze |
| 2026-11-20 | La rotta `export` convive con `/api/todos/{id:int}` grazie al vincolo `int` | bot (piano) | Verificato in `Program.cs` |
