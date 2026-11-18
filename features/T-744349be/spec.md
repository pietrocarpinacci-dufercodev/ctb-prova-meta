# T-744349be — Sezione "Prova cartella persistente" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-744349be: «Aggiungi al README del meta-repo una sezione "Prova cartella persistente" con una riga di testo. Non toccare i submodule.»

## Criteri di accettazione
- WHEN la feature viene eseguita, il meta-repo SHALL contenere in `README.md` un titolo di sezione `## Prova cartella persistente`, posto in fondo al file dopo il blocco `git clone`.
- WHEN la sezione viene aggiunta, il `README.md` SHALL contenere sotto il titolo la riga «Sezione di prova per verificare la cartella persistente del bot.» seguita dalla riga «followup 1» (aggiunta nel followup 1).
- WHEN la feature viene eseguita, il contenuto preesistente di `README.md` SHALL restare invariato.
- IF il lavoro richiederebbe modifiche ai submodule `api` o `web` (codice o puntatori) THEN il sistema SHALL non modificarli.
