# HANDOFF · Tesoreria chiese (programma-tesoreria)

Ultimo aggiornamento: 15 novembre 2026 · Versione 16.1

## Il progetto
App web a file unico (`index.html` + `sw.js`) per la tesoreria della chiesa. Usata soprattutto su iPad
(Safari / icona Home). Dati nel browser (state/persist), nessun server. Ricevute, casse, contributo
mensile, rapporti al Campo, analisi.

## Fatto in questa sessione (v16.1)
1. **Finestra ricevuta piccola dopo il salvataggio (iPad)**: salvando, il campo col fuoco spariva con
   la finestra e Safari non mandava `focusout`, quindi `--vvh` restava quella della tastiera.
   - `closeModal()` ora fa `blur()` del campo e chiama `rimisuraSchermo()`.
   - Nel blocco `visualViewport` (cerca «QUANTO SCHERMO RESTA SOPRA LA TASTIERA») `misura()` usa
     tutto lo schermo se nessun campo di testo ha il fuoco; in più rimisura ogni 300 ms finché c'è
     una `.modal.show`.
2. **Importo automatico sulle righe del Contributo (solo Entrata)**: nuova `compilaImporto(riga)`
   = `rate()` (quota in ✅ Contributo) × mesi scelti × persone (intestatario + «+Nome» nella
   descrizione, via `personeContributo`). Si ricalcola a: scelta cassa (`syncLine`), cambio mesi
   (`giraMese`, `svuotaMesi`), scrittura descrizione, cambio nome (`rinfrescaMesi`).
   Non sovrascrive cifre digitate a mano (`dataset.auto`) né ricevute già registrate riaperte.
   Test: 1 mese=25, 4 mesi=100, 4 mesi + «+lili»=200.
3. Versione portata a 16.1.

## Da sapere
- La cassa «Sede» o «Contributo…» è riconosciuta da `cassaContributoNome`/`cassaContributo`.
- Nessun HANDOFF precedente nel repo. La sessione non ha potuto verificare su un vero iPad con tastiera:
  **da provare** (salva una ricevuta con tastiera aperta e controlla che la finestra si riallunghi).
- `sw.js` cache `tesoreria-v2`: invariata.

## Prossimi passi / domande aperte
- Provare su iPad i punti 1 e 2.
- Quota diversa per ospiti/esenti? Ora ogni persona paga la quota piena.
