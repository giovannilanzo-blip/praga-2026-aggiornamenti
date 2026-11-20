# Aggiornamenti di viaggio · schema del file `aggiornamenti.json`

File pubblico letto dall'app quando il telefono ha internet. Contiene solo informazioni turistiche pubbliche (orari, linee dei mezzi, prezzi, chiusure). Nessun dato personale.

```json
{
  "schema": 1,
  "viaggio": { "da": "2026-12-05", "a": "2026-12-09", "citta": "Praga" },
  "generato": "2026-12-03T06:05:00Z",
  "ultimoControllo": "2026-12-03T06:05:00Z",
  "controlli": 42,
  "esito": "ok | parziale | errore",
  "messaggio": "Nessuna novità rispetto al controllo precedente.",
  "novita": [
    {
      "id": "agg-20261203-1",
      "pubblicata": "2026-12-03T06:05:00Z",
      "gravita": "alta | media | bassa",
      "giorno": "2026-12-07",
      "riguarda": { "tipo": "tappa | luogo | biglietto | generale", "id": "d7-salita" },
      "titolo": "Tram 22 deviato",
      "testo": "Testo breve con cosa cambia e cosa fare.",
      "patch": [ { "campo": "transport[1].lines", "valore": "23" } ],
      "fonte": "https://pid.cz/...",
      "superata": false
    }
  ],
  "cambio": { "eurCzk": 24.43, "data": "2026-12-03", "fonte": "https://www.cnb.cz/..." },
  "verificatoIl": { "d7-salita": "2026-12-03T06:05:00Z" },
  "domaniDaControllare": {
    "2026-12-07": [ { "id": "d7-castello", "testo": "Cambio della guardia alle 12:00: confermare l'orario sul sito del Castello." } ]
  },
  "stati": {
    "d6-planetario": {
      "giorno": "2026-12-06",
      "etichette": [ { "stato": "ultimi-posti", "testo": "Ultimi 6 posti alle 16:30" } ],
      "nota": "Replica di To Mars! delle 16:30.",
      "prenota": "https://www.planetum.cz/...",
      "fonte": "https://www.planetum.cz/...",
      "controllato": "2026-12-05T11:05:00Z"
    }
  }
}
```

- `riguarda.id` usa gli identificativi delle tappe (`d6-zoo`, `d7-salita`…), dei luoghi del catalogo (`cat-…`) o dei biglietti dell'app; `generale` per avvisi che valgono per tutto il viaggio (scioperi, meteo).
- `patch` è facoltativo: se presente, l'app propone di applicare la modifica al campo indicato della tappa; l'utente decide sempre.
- Le voci non si cancellano: quando non valgono più, `"superata": true`.
- `cambio` (facoltativo): cambio euro/corona usato dal convertitore dell'app, che non è modificabile a mano. Valore accettato tra 10 e 60; `data` è il giorno del cambio ufficiale.
- `stati` (facoltativo, letto dall'app dalla versione 1.0.9): stato di ristoranti, musei e attrazioni dell'itinerario e del catalogo, per id (`d6-planetario`, `cat-…`). L'app lo mostra come etichette colorate sulla scheda della tappa e nel riquadro «Aperture e posti liberi» della pagina «Da controllare».
  - `etichette`: 1 o 2 voci `{stato, testo}`; `testo` al massimo 40 caratteri, senza HTML (es. «Aperto 9:00–16:00», «Pieno domenica 6», «Posti martedì 8»).
  - `stato` e colore nell'app: `aperto` (verde) · `prenotabile` (blu: ci sono posti) · `ultimi-posti` e `orario-speciale` (giallo) · `pieno`, `chiuso`, `fermo` (rosso: esaurito, chiuso o impianto non in funzione) · `senza-prenotazione` (grigio) · `non-verificato` (tratteggiato: la fonte non dice nulla).
  - `controllato` (obbligatorio): istante ISO del controllo, mai nel futuro. `giorno`: giorno di viaggio a cui si riferisce (passato quel giorno l'app non lo mostra più). `nota`: al massimo 200 caratteri. `prenota` e `fonte`: indirizzi https dei siti ufficiali o di prenotazione già presenti nei dati dell'app (gli altri vengono ignorati).
  - «pieno» solo se il sistema di prenotazione del luogo mostra il tutto esaurito: l'affollamento in tempo reale non va stimato.
  - L'app scarta le voci non valide; un file senza `stati` resta valido.
