# T-d2380836 — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-d2380836: «Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule.» Il branch di partenza del meta-repo è `e2e-base`.

## Criteri di accettazione
- WHEN si completa l'execute, il `README.md` del meta-repo SHALL contenere un titolo di sezione `## Prova branch di partenza`.
- WHEN si completa l'execute, la sezione `## Prova branch di partenza` SHALL contenere esattamente una riga di testo non vuota.
- WHEN si completa l'execute, il resto del `README.md` SHALL restare invariato.
- IF il commit dell'execute contiene file nei submodule `api` o `web`, o i loro puntatori, THEN il job SHALL essere considerato non conforme.
