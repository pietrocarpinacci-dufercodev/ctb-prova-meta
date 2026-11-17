# T-a71d567b — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo.

## Passi
1. In `README.md` del meta-repo aggiungi in fondo, dopo il blocco `git clone`, una riga vuota e la sezione:
   ```markdown
   ## Prova cartella persistente

   <riga di testo, vedi D1>
   ```
2. Verifica con `git diff` che cambi solo `README.md` (nessuna riga esistente modificata) e che `git submodule status` non mostri puntatori spostati.
3. Committa nel meta-repo solo `README.md` con messaggio `[T-a71d567b] Sezione "Prova cartella persistente" nel README`.
4. Scrivi `/work/out/meta.md` con la descrizione della PR; nessuna descrizione per i submodule, che non sono toccati.

Build e test: non servono (solo documentazione); `dotnet test api` e `npm test --prefix web` non sono interessati dalla modifica.

## Fonti
- Richiesta nel thread Teams della feature T-a71d567b.
- `README.md` del meta-repo (struttura attuale).
- `CLAUDE.md` del meta-repo (struttura, comandi, regola sull'agente `backend-dev`).
- `features/T-9dfe1d36/decisions.md`: nessun conflitto (riguardano l'agente `backend-dev`).
- Nessun allegato in `/work/inputs`.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, un solo file Markdown.
- Sicurezza: nessun impatto, nessun codice né segreto coinvolto.
- Accoppiamento: nessuno, i submodule e il contratto API/web non cambiano.

## Domande aperte
- [ ] D1: Quale testo va nella riga della sezione? Proposta del bot se non indicato: «Questa sezione verifica che la cartella di lavoro del bot resti persistente tra un job e l'altro.» — fonti: richiesta nel thread ("una riga di testo", contenuto non specificato) vs criteri di accettazione della spec
