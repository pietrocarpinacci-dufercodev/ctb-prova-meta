# T-a71d567b — Sezione "Prova cartella persistente" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-a71d567b (link non disponibile nel job): «Aggiungi al README del meta-repo una sezione "Prova cartella persistente" con una riga di testo. Non toccare i submodule.»

## Criteri di accettazione
- WHEN la feature è completata, il `README.md` del meta-repo SHALL contenere un titolo di sezione `## Prova cartella persistente`.
- WHEN la feature è completata, la sezione "Prova cartella persistente" SHALL contenere esattamente una riga di testo non vuota.
- WHEN la feature è completata, il contenuto esistente di `README.md` SHALL restare invariato, salvo l'aggiunta della nuova sezione in fondo al file.
- IF la feature è completata THEN i submodule `api` e `web` SHALL avere lo stesso puntatore e lo stesso contenuto del branch di partenza `develop`.
