# T-1a23573b — Riga con la data nel README

## Richiesta
Il team ha chiesto nel thread Teams: «Aggiungi al README del meta-repo una riga con la data di oggi.»

## Criteri di accettazione
- WHEN il job di execute termina, il `README.md` del meta-repo SHALL contenere una nuova riga con la data (formato e posizione da D1, D2, D3).
- IF la data non è determinabile dal sistema THEN l'esecuzione SHALL fermarsi e segnalarlo, senza inventare una data.
- WHILE il job modifica il README, il contenuto esistente SHALL restare invariato.
- WHERE la modifica tocca solo il meta-repo, i submodule `api` e `web` SHALL restare invariati.
