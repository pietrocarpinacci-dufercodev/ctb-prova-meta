# T-443cf26e — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md` aggiungere in fondo la sezione `## Prove automatiche` con due righe di testo: il canale di prova riceve messaggi del programma `ctb-e2e` (nome scritto tra apici inversi, come da richiesta del thread; testo esatto da confermare, vedi D1).
2. Verificare che il diff tocchi solo `README.md` e `features/T-443cf26e/`.

## Fonti
- Richiesta del thread.
- `README.md` e `CLAUDE.md` del meta-repo (nessun riferimento esistente a ctb-e2e).

## Rischio
Livello: verde
- Costi: trascurabili, modifica di documentazione.
- Complessità: minima, due righe in un file.
- Sicurezza: nessuna; non citare token, URL del canale o altri segreti.
- Accoppiamento: nessuno, submodule non toccati.

## Domande aperte
- [ ] D1: Quale canale è "il canale di prova" (nome o riferimento da citare) e ctb-e2e va descritto più nel dettaglio (es. link al repo)? Senza dati, il testo resterà generico — fonti: richiesta vs README (nessuna informazione sul canale)
