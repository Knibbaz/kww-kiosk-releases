# KWW Kiosk — releases & web-flasher

**Publieke** repo met de gecompileerde firmware van de KWW Kiosk (KWWK) en een
browser-flasher, zodat een kastje geflasht en bijgewerkt kan worden zonder de
(privé) broncode. De broncode staat in `knibbaz/kww-kiosk`.

## Wat hier staat

| Bestand | Wat |
|---|---|
| `index.html` | Web-flasher (ESP Web Tools, Web Serial). Flasht een kastje via USB vanuit de browser. |
| `manifest.json` | Manifest dat de flasher vertelt welke binary te schrijven. |
| `feedback-kiosk-esp32s3.bin` | De samengevoegde firmware-binary (bootloader + partities + app). |
| `survey/` | Losse feedback/review-landingspagina. |

## Web-flasher gebruiken

Web Serial werkt alleen op `https://` of `localhost`. Zet deze repo als
**GitHub Pages** aan (Settings → Pages → deploy from branch `main`, root), dan is
de flasher bereikbaar op `https://knibbaz.github.io/kww-kiosk-releases/`.

Lokaal testen:

    python3 -m http.server 8000
    # open http://localhost:8000

Sluit een kastje via USB-C aan, klik **Connect** en kies de poort.

## Nieuwe firmware publiceren

De binary komt uit de firmware-repo (`pio run -e esp32-s3-amoled-175` +
`scripts/build_webflasher.sh`, die de merged `.bin` maakt). Publiceren = de nieuwe
`feedback-kiosk-esp32s3.bin` en een bijgewerkte `manifest.json` hierheen kopiëren
en committen. (Later automatiseren we dit vanuit de CI van de firmware-repo; dat
vraagt een deploy-token — zie de TODO in `kww-kiosk`.)
