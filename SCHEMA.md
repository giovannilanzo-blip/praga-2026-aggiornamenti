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
  "verificatoIl": { "d7-salita": "2026-12-03T06:05:00Z" },
  "domaniDaControllare": {
    "2026-12-07": [ { "id": "d7-castello", "testo": "Cambio della guardia alle 12:00: confermare l'orario sul sito del Castello." } ]
  }
}
```

- `riguarda.id` usa gli identificativi delle tappe (`d6-zoo`, `d7-salita`…), dei luoghi del catalogo (`cat-…`) o dei biglietti dell'app; `generale` per avvisi che valgono per tutto il viaggio (scioperi, meteo).
- `patch` è facoltativo: se presente, l'app propone di applicare la modifica al campo indicato della tappa; l'utente decide sempre.
- Le voci non si cancellano: quando non valgono più, `"superata": true`.
