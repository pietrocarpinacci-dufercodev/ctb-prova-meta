# T-c0f1fe3d — Revisione del README del submodule api

## Richiesta
Il team chiede (thread Teams del job T-c0f1fe3d) di usare gli agenti definiti in `.claude/agents` del meta-repo per una breve revisione del README del primo submodule (`api`, primo in `.gitmodules`): punti poco chiari e informazioni mancanti, al massimo 5 punti. L'esito va in `docs/revisione-readme.md` del meta-repo. Nella risposta finale vanno scritti esattamente i nomi degli agenti usati.

## Criteri di accettazione
- WHEN la revisione è eseguita, il sistema SHALL usare un agente tra quelli definiti in `.claude/agents` (oggi solo `backend-dev`; se ne esistessero due o più, due agenti diversi).
- WHEN la revisione è eseguita, il sistema SHALL leggere solo `api/README.md` e non modificare il codice dei submodule.
- WHEN la revisione termina, il sistema SHALL creare `docs/revisione-readme.md` nel meta-repo con al massimo 5 punti, ciascuno classificato come "poco chiaro" o "mancante".
- IF `docs/revisione-readme.md` esiste già THEN il sistema SHALL mantenerne il contenuto e aggiungere la nuova revisione, senza sovrascriverlo.
- WHEN il job termina, il sistema SHALL riportare nella risposta finale il nome esatto di ogni agente usato.
- IF l'agente non può essere invocato THEN il sistema SHALL scriverlo nell'esito, senza simulare la revisione.
