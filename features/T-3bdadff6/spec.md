# T-3bdadff6 — Revisione del README del submodule api con un agente

## Richiesta
Il team ha chiesto, nel thread Teams della feature T-3bdadff6, di usare un agente di `.claude/agents` del meta-repo (due diversi se ce ne sono due o più) per una breve revisione del README del primo submodule (punti poco chiari e informazioni mancanti, max 5 punti), con esito in `docs/revisione-readme.md` e, nella risposta finale, il nome esatto degli agenti usati.

## Criteri di accettazione
- WHEN l'execute avvia la revisione, il bot SHALL delegarla a un agente definito in `.claude/agents` tramite il tool Agent.
- WHERE `.claude/agents` contiene un solo agente, il bot SHALL usare solo quell'agente (`backend-dev`).
- WHERE `.claude/agents` contiene due o più agenti, il bot SHALL usarne due diversi.
- WHEN la revisione è completata, il bot SHALL scrivere `docs/revisione-readme.md` con al massimo 5 punti, ciascuno "poco chiaro" o "informazione mancante", riferiti a `api/README.md`.
- IF il tool Agent non è disponibile THEN il bot SHALL lavorare senza agente e scriverlo nell'esito.
- WHEN il bot risponde a fine job, la risposta finale SHALL riportare esattamente il nome degli agenti usati.
- IF un percorso di output è sotto `docs/superpowers/` THEN il bot SHALL NOT scriverci.
- Il bot SHALL NOT modificare file nei submodule.
