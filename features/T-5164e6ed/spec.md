# T-5164e6ed — Scadenza dei todo

## Richiesta
Il team chiede (thread Teams del job T-5164e6ed) di aggiungere una scadenza ai todo: nell'API un campo facoltativo `dueDate` (solo data) in creazione e in lettura, e un endpoint per elencare i todo scaduti; nel web la data accanto a ogni todo, evidenziata se è passata. Il team non sa ancora se i todo già completati debbano comparire tra gli scaduti (vedi D1 in `plan.md`).

## Criteri di accettazione
- WHEN un client invia `POST /api/todos` con `dueDate` nel formato `yyyy-MM-dd`, l'API SHALL rispondere 201 con il todo che contiene lo stesso `dueDate`.
- WHEN un client invia `POST /api/todos` senza `dueDate`, l'API SHALL creare il todo con `dueDate` nullo, come oggi.
- WHEN un client legge un todo (`GET /api/todos`, `GET /api/todos/{id}`, `POST /api/todos/{id}/done`), l'API SHALL includere `dueDate` (data `yyyy-MM-dd` oppure `null`).
- IF `dueDate` non è una data valida `yyyy-MM-dd` THEN l'API SHALL rispondere 400 e non creare il todo.
- WHEN un client chiama l'endpoint dei scaduti, l'API SHALL elencare i todo non completati con `dueDate` precedente alla data di oggi, ordinati per id.
- WHERE un todo non ha `dueDate`, l'API SHALL escluderlo dall'elenco dei scaduti.
- WHEN un todo ha `dueDate` uguale a oggi, l'API SHALL non considerarlo scaduto.
- WHEN il web mostra un todo con `dueDate`, la pagina SHALL mostrare la data accanto al titolo.
- WHEN il web mostra un todo non completato con `dueDate` passata, la pagina SHALL evidenziare la data.
- WHERE un todo non ha `dueDate`, la pagina SHALL mostrare solo il titolo.
- WHEN i todo vengono creati senza `dueDate` (client esistenti), il comportamento esistente di API e web SHALL restare invariato.
- Pendente su D1: se i todo completati debbano comparire tra gli scaduti (i criteri sopra assumono di no) e se vadano evidenziati nel web.
