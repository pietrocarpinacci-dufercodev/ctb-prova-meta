# T-4b10e595 — Sezione "Prova cartella persistente" nel README del meta-repo

## Richiesta
Dal thread Teams della feature T-4b10e595: «Aggiungi al README del meta-repo una sezione "Prova cartella persistente" con una riga di testo. Non toccare i submodule.»

## Criteri di accettazione
- Il `README.md` del meta-repo SHALL contenere un titolo di sezione `## Prova cartella persistente`.
- La sezione `## Prova cartella persistente` SHALL contenere la riga di testo iniziale seguita dalle righe `followup 1` e `followup 2`, ognuna in un paragrafo separato (decisioni del 2026-11-18, followup).
- WHEN la sezione viene aggiunta, il resto del contenuto di `README.md` SHALL restare invariato.
- IF la modifica tocca file diversi da `README.md` e `features/T-4b10e595/` THEN la modifica SHALL essere considerata fuori perimetro.
- Il cambio SHALL lasciare invariati i puntatori dei submodule `api` e `web`.
