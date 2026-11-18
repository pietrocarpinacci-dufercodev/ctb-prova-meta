# T-333ed6a2 — Decisioni

| Data | Decisione | Chi | Motivo |
| --- | --- | --- | --- |
| 2026-11-18 | Intestazione e formato del CSV presi da `dati.csv`: `titolo,priorita`, virgola, LF, UTF-8 | bot (piano) | Unico esempio di formato fornito; le intestazioni sono dati, non codice, quindi restano in italiano |
| 2026-11-18 | Quoting dei campi secondo RFC 4180, senza dipendenze nuove (`StringBuilder` e helper di escape in `Program.cs`) | bot (piano) | I titoli liberi possono contenere virgole o doppi apici; il formato standard basta |
| 2026-11-18 | `immagine-1.png` non usata come fonte finché non viene chiarita (D1) | bot (piano) | L'immagine è tutta verde e non contiene nessuno schema leggibile |
