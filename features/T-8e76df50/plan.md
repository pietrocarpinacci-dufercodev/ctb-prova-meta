# T-8e76df50 — Piano

## Submodule coinvolti
Nessuno: la modifica riguarda solo il `README.md` del meta-repo.

## Passi
1. Sul branch `claude/T-8e76df50` del meta-repo, aggiungere in fondo a `README.md` (dopo il blocco `git clone`) una riga vuota, il titolo `## Prova branch di partenza` e una riga di testo (testo proposto: "Sezione aggiunta per verificare che il bot parta dal branch `e2e-base` del meta-repo invece che da `develop`.", salvo risposta diversa a D1).
2. Verificare con `git diff` e `git status` che cambino solo `README.md` e i file della feature, e che i puntatori di `api` e `web` non siano nel commit.
3. Committare con il messaggio `[T-8e76df50] Sezione "Prova branch di partenza" nel README`.
4. Scrivere la descrizione della PR in `/work/out/meta.md`. Non servono build né test: la modifica riguarda solo la documentazione.

## Fonti
- Richiesta nel thread Teams (messaggio del job plan).
- `README.md` del meta-repo.
- `CLAUDE.md` del meta-repo (struttura, branch base).
- `features/*/decisions.md`: nessuna decisione riguarda il README o i branch di partenza.
- Nessun allegato in `/work/inputs` usato.

## Rischio
Livello: verde
- Costi: trascurabili, una riga di documentazione.
- Complessità: minima, nessun codice.
- Sicurezza: nessun impatto.
- Accoppiamento: nessuno, i submodule restano invariati.

## Domande aperte
- [ ] D1: La richiesta non dice quale testo mettere nella riga: va bene il testo proposto al passo 1, o ne volete un altro? — fonti: richiesta nel thread ("una riga di testo") vs spec.md (criterio sul contenuto della sezione)
