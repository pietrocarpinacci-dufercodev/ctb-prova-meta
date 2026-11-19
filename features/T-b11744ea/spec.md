# T-b11744ea — Sezione "Prova chiusura thread" nel README del meta-repo

## Richiesta
Nel thread Teams il team chiede: "Aggiungi al README del meta-repo una sezione 'Prova chiusura thread' con una riga di testo. Non toccare i submodule." Il messaggio del job non riporta il link al thread né il testo della riga: il testo è aperto (vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN il job di execute è approvato e la domanda D1 è risolta, il sistema SHALL aggiungere a `README.md` una sezione intitolata `## Prova chiusura thread`.
- WHEN la sezione viene aggiunta, il sistema SHALL includere sotto il titolo una sola riga di testo, con il contenuto indicato in D1.
- WHEN la sezione viene aggiunta, il sistema SHALL collocarla in fondo a `README.md`, dopo il blocco di comando di clone.
- WHEN la sezione viene aggiunta, il sistema SHALL lasciare invariato il resto del contenuto di `README.md`.
- WHILE il job è in corso, il sistema SHALL non modificare alcun file dei submodule `api/` e `web/`.
- IF la modifica produce differenze fuori da `README.md` (esclusi gli eventuali puntatori di submodule che le regole del bot gestiscono) THEN il sistema SHALL fermarsi e segnalarlo nell'esito.
- IF D1 non è ancora risolta THEN il sistema SHALL non scrivere la riga di testo inventandone il contenuto e SHALL lasciare la domanda aperta.
