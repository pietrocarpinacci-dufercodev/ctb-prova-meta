# T-64dfd7fa — Piano

## Submodule coinvolti
- Nessuno: la modifica riguarda solo `README.md` del meta-repo. I submodule `api` e `web` non si toccano.

## Passi
1. Aprire `README.md` nel meta-repo (branch `claude/T-64dfd7fa`, partenza `develop`).
2. Aggiungere in fondo al file, dopo il blocco di clone, una riga vuota seguita da `## Prova chiusura thread` e da una riga vuota.
3. Scrivere come unico contenuto della sezione la riga `Thread chiuso e riaperto da ctb-e2e.`.
4. Lasciare invariato il resto del file (titolo, descrizione, blocco `bash` di clone).
5. Verificare con lettura del diff che: il file modificato è solo `README.md`; non ci sono modifiche a `api` o `web`; la sezione compare una sola volta.
6. Aggiornare `features/T-64dfd7fa/decisions.md` con le decisioni di posizione e livello di heading.

## Fonti
- Richiesta nel thread Teams (testo del job plan).
- `README.md` del meta-repo (struttura attuale: titolo `#`, descrizione, blocco bash di clone).
- `CLAUDE.md` del meta-repo: nessuna regola specifica sul README; nessun comando di build richiesto per un cambio di sola documentazione.
- `features/_template/*` e `features/T-9dfe1d36/decisions.md` per il formato dei file della feature.
- Allegati in `/work/inputs`: nessuno usato.

## Rischio
Livello: verde
- Costi: una riga di testo più un heading in un unico file; nessun costo di build o di runtime.
- Complessità: nulla; nessuna logica, nessuna dipendenza.
- Sicurezza: nessun impatto; il contenuto è testo fisso e non contiene segreti.
- Accoppiamento: nessuno; nessun submodule né file di codice coinvolto.

## Domande aperte
<!-- Nessuna domanda aperta: la posizione (fondo del file) e il livello di heading (##) sono scelte del piano, riportate in decisions.md. -->
