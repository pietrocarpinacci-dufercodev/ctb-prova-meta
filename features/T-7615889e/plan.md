# T-7615889e — Piano

## Submodule coinvolti
- api: nuovo endpoint `GET /api/todos/export` in `src/CtbProva.Api/Program.cs` (CSV con colonne `titolo,priorita`), test in `tests/CtbProva.Api.Tests/TodoApiTests.cs`, riga nuova nella tabella endpoint di `README.md`.
- web: nessuna modifica prevista (l'endpoint è nuovo e non cambia il contratto esistente). Cambia solo se D1 introduce il campo `priorita` in `Todo`: allora si aggiornano `web/src/todos.js` e i test.

## Passi
1. Risolvere le domande aperte D1–D4 (bloccanti per il formato).
2. Test per primi (`TodoApiTests.cs`): `GET /api/todos/export` risponde 200 con `Content-Type: text/csv`; intestazione `titolo,priorita`; una riga per todo ordinata per id; elenco vuoto → solo intestazione; titoli con virgola, virgolette o a capo escapati (RFC 4180).
3. Implementare in `Program.cs` un `MapGet("/api/todos/export", ...)` che restituisce `Results.Text(csv, "text/csv")`, con escape dei campi. Il vincolo `{id:int}` della rotta esistente evita il conflitto con `export`.
4. Se D1 = aggiungere la priorità: estendere `Todo`/`NewTodo`/`TodoStore.Add`, validare i valori `bassa|media|alta` (400 altrimenti) e allineare `web/src/todos.js` e i test di entrambi, come richiede il CLAUDE.md per i cambi di contratto.
5. Aggiornare `api/README.md` con l'endpoint.
6. `dotnet test api` (e `npm test --prefix web` se il web è toccato).

## Fonti
- Allegato `/work/inputs/requisiti.pdf`: unico requisito leggibile: «la priorita ha i valori bassa, media e alta».
- Allegato `/work/inputs/dati.csv`: formato di riferimento: intestazione `titolo,priorita`, separatore virgola, valori senza virgolette, due righe di esempio (`Comprare il latte,bassa`, `Preparare la demo,alta`).
- Allegato `/work/inputs/immagine-1.png`: immagine di 1 KB, in pratica un singolo pixel quasi bianco/verde, senza schema leggibile. Non ho potuto ricavarne informazioni (vedi D3).
- `api/src/CtbProva.Api/Program.cs`: `Todo(Id, Title, Done)`, nessun campo priorità; rotte esistenti.
- `api/README.md`, `CLAUDE.md` del meta-repo (contratto API/web da tenere allineato).
- Non esistono `decisions.md` di altre feature (solo `features/_template`).

## Rischio
Livello: verde
- Costi: basso, un endpoint di sola lettura e pochi test. Sale a medio se D1 aggiunge il campo priorità.
- Complessità: bassa; l'unico punto delicato è l'escape CSV.
- Sicurezza: titoli inseriti dagli utenti finiscono in un CSV: rischio di formula injection se aperto in Excel (D4). Nessun segreto coinvolto.
- Accoppiamento: nullo con l'endpoint solo-lettura; con la priorità tocca contratto API, web e test di entrambi.

## Domande aperte
- [ ] D1: `Todo` oggi non ha il campo priorità, ma `dati.csv` e il PDF la richiedono come colonna con valori bassa/media/alta. Va aggiunta al modello (con default, ad esempio `media`, e campo opzionale in creazione) o la colonna va esportata vuota? — fonti: requisiti.pdf e dati.csv vs api/Program.cs
- [ ] D2: Formato esatto: intestazione `titolo,priorita` (senza accento, come in dati.csv) e nessuna colonna `id` né `done`? Servono altre colonne? Separatore virgola, fine riga `\n` o `\r\n`, codifica UTF-8 con o senza BOM? — fonti: dati.csv vs richiesta
- [ ] D3: L'immagine allegata non contiene uno schema leggibile (circa 1 pixel). Si può ricaricare l'immagine corretta o descrivere lo schema a parole? — fonti: immagine-1.png vs richiesta
- [ ] D4: I titoli che iniziano con `=`, `+`, `-`, `@` vanno neutralizzati (prefisso `'`) contro la formula injection in Excel, oppure l'esportazione deve restare fedele ai dati? — fonti: richiesta vs buone pratiche CSV
- [ ] D5: L'export include tutti i todo (anche quelli fatti)? Serve `Content-Disposition: attachment; filename=todos.csv`? — fonti: richiesta vs api/Program.cs
