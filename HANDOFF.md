# HANDOFF · Tesoreria chiese (programma-tesoreria)

Ultimo aggiornamento: 15 novembre 2026 · Versione 16.2

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

## v16.2 · Scansione documenti
- Qualità massima: `SCAN_LATO` 4400, `SCAN_QUALITA` .97, fotocamera ideal 4096x3072, scatto fino a 5200 px;
  dove c'è `ImageCapture` (Chrome/Android) si usa la foto vera, su iPad il fotogramma video.
- Default **Colore** (primo pulsante); «Grigi» e «Bianco e nero» restano.
- Ombre: in `scanRifinisci` stima della luce più fine (110 px, max r3 + due sfocature).
- Inquadratura: `scanTrovaQuad` a 900 px (live 320) e **seconda soglia** per il foglio in ombra
  (gruppo neutro, 1.8x più chiaro del tavolo) così l'ombra non taglia il foglio. Test sintetico: angoli esatti.
- Non provato su iPad reale.

## Da sapere
- La cassa «Sede» o «Contributo…» è riconosciuta da `cassaContributoNome`/`cassaContributo`.
- Nessun HANDOFF precedente nel repo. La sessione non ha potuto verificare su un vero iPad con tastiera:
  **da provare** (salva una ricevuta con tastiera aperta e controlla che la finestra si riallunghi).
- `sw.js` cache `tesoreria-v2`: invariata.

## Prossimi passi / domande aperte
- Provare su iPad i punti 1 e 2.
- Quota diversa per ospiti/esenti? Ora ogni persona paga la quota piena.
