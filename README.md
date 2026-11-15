# Lima Llama 🦙

Eine Weltkarte, auf der ein Lama zwischen Orten hin- und herhüpft, an denen Günther verhaftet, des Landes verwiesen oder auf andere Weise aktenkundig wurde. Unterwegs denkt das Lama in einer Gedankenwolke an den nächsten Ort. Dort erscheint ein Ortsschild, und das Lama sagt etwas in der Sprechblase. Gibt es für die Station eine Akte, erstarrt das Lama, ein roter BUSTED-Stempel knallt aufs Bild und die Akte mit den Verurteilungen öffnet sich. Im Finale läutet eine Glocke, das Lama verblasst, ein Grabstein mit den Lebensdaten erscheint, danach beginnt die Runde von vorn.

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

| Feld      | Bedeutung |
|-----------|-----------|
| `name`    | „Ort, Land“. Der Ort steht groß auf dem Ortsschild, das Land klein darunter |
| `lng`     | Längengrad |
| `lat`     | Breitengrad |
| `year`    | Jahreszahl: Ortsschild, Zeitleiste, Akte, Grabstein (erstes und letztes Jahr) |
| `reason`  | optional, z. B. „verhaftet“, steht klein auf dem Ortsschild |
| `thought` | optional, Gedanke **auf dem Weg zu dieser Station** (Gedankenwolke) |
| `say`     | optional, Satz des Lamas **an dieser Station** (Sprechblase) |
| `akte`    | optional, Liste von Verurteilungen. Wenn vorhanden: BUSTED-Stempel + Akte |

Jeder Eintrag in `akte` hat die Felder `datum`, `behoerde`, `vorwurf`, `urteil`. Leere Felder werden nicht angezeigt:

```json
"akte": [
  { "datum": "12.03.1976", "behoerde": "Landesgericht Salzburg", "vorwurf": "…", "urteil": "…" }
]
```

Die Akte bleibt je nach Textlänge mindestens 5 Sekunden offen, ein Klick schließt sie früher. Löschst du `akte` bei einer Station (oder lässt die Liste leer), gibt es dort keinen Stempel.

Wie lange Gedanke und Satz zu sehen sind, richtet sich automatisch nach der Textlänge. Platzhalter stehen in eckigen Klammern, z. B. `[Satz in Lima, 1976]`.

Die letzte Station löst das Finale aus.

## Mapbox-Token

Der Token in `index.html` ist ein öffentlicher `pk.`-Token und im Browser ohnehin sichtbar. Er sollte im [Mapbox-Dashboard](https://account.mapbox.com/access-tokens/) auf die Domains beschränkt sein, auf denen die Seite läuft (z. B. `https://<user>.github.io/*` und `http://localhost:*`).

## Dateien

- `index.html`: die gesamte Anwendung (Mapbox GL JS + Canvas-Overlay)
- `locations.json`: Stationen
- `humming.m4a`, `bell.mp3`, `166625__jzazvurek__childs-scream-8.wav`: Ton

Quellen der Medien: siehe [CREDITS.md](CREDITS.md). Lizenz des Codes: siehe [LICENSE](LICENSE).
