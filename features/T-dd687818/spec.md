# T-dd687818 — Sezione "Prova chiusura thread" nel README del meta-repo

## Richiesta
Nel thread Teams (link non disponibile nel job) il team chiede di aggiungere al README del meta-repo una sezione "Prova chiusura thread" con la sola riga "Thread chiuso e riaperto da ctb-e2e." Niente submodule, niente altro.

## Criteri di accettazione
- WHEN il job di execute è completato, il sistema SHALL contenere in `README.md` (meta-repo) una sezione di secondo livello intitolata `## Prova chiusura thread`.
- WHEN la sezione è presente, il sistema SHALL riportarvi come unico contenuto testuale la riga `Thread chiuso e riaperto da ctb-e2e.`, senza altre righe di testo.
- WHEN la sezione è aggiunta, il sistema SHALL lasciare invariato il testo già presente in `README.md`.
- IF la sezione `## Prova chiusura thread` esiste già in `README.md` THEN il sistema SHALL aggiornarla e non crearne una seconda.
- WHILE il job di execute è in corso, il sistema SHALL non modificare file dei submodule `api/` e `web/` e non modificare file del meta-repo diversi da `README.md` e da `features/T-dd687818/`.
- WHILE il job di execute è in corso, il sistema SHALL non eseguire push, PR, merge o cancellazioni di branch.
