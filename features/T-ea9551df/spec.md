# T-ea9551df — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-ea9551df: «Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule.»

Branch di partenza del job: meta-repo `e2e-base`, `api` `e2e-base`, `web` `develop`.

Testo proposto per la riga (approvandolo, si approva anche il testo):

> Sezione aggiunta dal bot per verificare il lavoro su un branch di partenza diverso da `develop`.

## Criteri di accettazione
- WHEN la feature è implementata, il `README.md` del meta-repo SHALL contenere un titolo di sezione `## Prova branch di partenza` in fondo al file.
- WHEN la feature è implementata, la sezione `## Prova branch di partenza` SHALL contenere esattamente una riga di testo, uguale al testo proposto sopra.
- WHEN la feature è implementata, il resto del `README.md` SHALL restare invariato.
- IF la modifica tocca un file nei submodule `api` o `web` o ne sposta il puntatore THEN la feature SHALL essere considerata non conforme.
