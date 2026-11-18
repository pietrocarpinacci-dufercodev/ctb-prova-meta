# T-9c625913 — Sezione "Prova branch di partenza" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-9c625913: "Aggiungi al README del meta-repo una sezione "Prova branch di partenza" con una riga di testo. Non toccare i submodule."

Branch di partenza: meta-repo → `e2e-base`, `api` → `e2e-base`, `web` → `develop`.

## Criteri di accettazione
- WHEN la feature è implementata, il `README.md` del meta-repo SHALL contenere un'intestazione di secondo livello `## Prova branch di partenza`, posta in fondo al file dopo il blocco `git clone`.
- WHEN la feature è implementata, la sezione `## Prova branch di partenza` SHALL contenere esattamente una riga di testo in italiano (proposta: "Sezione aggiunta per verificare che il bot parta dal branch indicato nel job invece che da `develop`.").
- WHEN la feature è implementata, il contenuto preesistente di `README.md` SHALL restare invariato.
- IF il commit della feature includesse modifiche ai file o ai puntatori dei submodule `api` e `web` THEN la feature SHALL essere considerata non conforme.
