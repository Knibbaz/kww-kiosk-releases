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

**Automatisch (aanrader):** in de firmware-repo (`kww-kiosk`) een tag pushen —
`.github/workflows/release.yml` bouwt de firmware, voegt hem samen en publiceert
hierheen:

    git tag v1.3.26 && git push origin v1.3.26

Eenmalige setup (in `kww-kiosk`, niet hier):

1. Maak op GitHub een **fine-grained personal access token** met alleen
   *Repository access: Only select repositories* -> deze repo
   (`kww-kiosk-releases`) en *Permissions* -> **Contents: Read and write**.
   Geen andere rechten, geen toegang tot andere repo's.
2. Zet dat token in `kww-kiosk` als repo-secret
   **`RELEASES_DEPLOY_TOKEN`** (Settings -> Secrets and variables -> Actions).
3. Klaar. Elke `v*`-tag in `kww-kiosk` publiceert automatisch hierheen; de
   workflow weigert te publiceren als de tag niet overeenkomt met `FW_VERSION`
   in `src/config.h`.

**Handmatig (zonder CI):** `pio run -e esp32-s3-amoled-175` +
`scripts/build_webflasher.sh` in de firmware-repo bouwt lokaal dezelfde
`feedback-kiosk-esp32s3.bin` + bijgewerkte `manifest.json`/`index.html` in deze
map (als sibling-checkout); commit en push die dan hier.
