# Lima Llama 🦙

Eine Weltkarte, auf der ein Lama zwischen Orten hin- und herhüpft, an denen Günther verhaftet, des Landes verwiesen oder auf andere Weise aktenkundig wurde. An jeder Station schreit das Lama die Jahreszahl. Im Finale läutet eine Glocke, das Lama verblasst, ein Grabstein mit den Lebensdaten erscheint, danach beginnt die Runde von vorn.

## Starten

Die Seite lädt `locations.json` per `fetch`. Deshalb funktioniert sie **nicht** per Doppelklick (`file://`), sondern nur über einen Webserver:

```bash
python3 -m http.server 8000
```

Danach http://localhost:8000 öffnen, warten, bis die Karte geladen ist, und klicken.

Alternativ: GitHub Pages für das Repo aktivieren (Settings → Pages → Branch `main`).

## Kiosk-Modus (Ausstellung)

Mit `?kiosk` in der URL startet die Animation ohne Klick, sobald alles geladen ist. Mauszeiger und Vollbild-Knopf sind dann ausgeblendet:

```
http://localhost:8000/?kiosk
```

Browser spielen Ton ohne Klick normalerweise nicht ab. Für einen Ausstellungsrechner Chrome so starten (macOS):

```bash
open -a "Google Chrome" --args --kiosk --autoplay-policy=no-user-gesture-required "http://localhost:8000/?kiosk"
```

Damit der Bildschirm nicht ausgeht, fordert die Seite einen Wake Lock an. Er funktioniert nur über `https://` oder `localhost`.

## Stationen bearbeiten

`locations.json` enthält eine Liste von Stationen in Reihenfolge:

| Feld     | Bedeutung                                                        |
|----------|------------------------------------------------------------------|
| `name`   | Ort, wird im Label angezeigt                                     |
| `lng`    | Längengrad                                                       |
| `lat`    | Breitengrad                                                      |
| `year`   | Jahreszahl, erscheint in der Sprechblase und im Label            |
| `reason` | optional, z. B. „verhaftet“, wird im Label nach der Jahreszahl angezeigt |

Die letzte Station löst das Finale aus.

## Mapbox-Token

Der Token in `index.html` ist ein öffentlicher `pk.`-Token und im Browser ohnehin sichtbar. Er sollte im [Mapbox-Dashboard](https://account.mapbox.com/access-tokens/) auf die Domains beschränkt sein, auf denen die Seite läuft (z. B. `https://<user>.github.io/*` und `http://localhost:*`).

## Dateien

- `index.html`: die gesamte Anwendung (Mapbox GL JS + Canvas-Overlay)
- `locations.json`: Stationen
- `humming.m4a`, `bell.mp3`, `166625__jzazvurek__childs-scream-8.wav`: Ton

Quellen der Medien: siehe [CREDITS.md](CREDITS.md). Lizenz des Codes: siehe [LICENSE](LICENSE).
