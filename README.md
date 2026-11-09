# Schulbus 646

Kleine Web-App für den Schulweg: Sie zeigt, wann man losgehen muss, um den Bus der Linie 646 von Hiddenhausen Richtung Bünde zu erreichen – mit Echtzeit-Verspätungen, Alternativen und der berechneten Position des Busses auf einer Karte.

Die App ist eine einzelne Datei (`index.html`), läuft im Browser auf iPhone, iPad und Computer und braucht keinen Server, keine Anmeldung und keinen API-Schlüssel.

## Funktionen

- **Countdown bis zum Losgehen:** „Los in 4:30 · Losgehen um 07:04“, berechnet aus der Echtzeit-Abfahrt minus Gehzeit.
- **Gehzeiten Schnell / Normal / Langsam:** frei einstellbar, täglich per Umschalter wählbar.
- **Verspätungen und Ausfälle:** Abfrage beim Öffnen der App, beim Zurückkehren in die App und per Knopf „Verspätung jetzt prüfen“.
- **Mehrere Ziele:** z. B. Bünde Bahnhof und Bünde Marktplatz. Berücksichtigt werden alle Busse, die von der eigenen Haltestelle direkt zu einem Ziel fahren, auch andere Linien.
- **Einsatzwagen:** Fahrten mit Zusatzbuchstaben (z. B. E646) werden der Linie zugeordnet und gekennzeichnet, sofern sie im Fahrplan stehen.
- **Alternativen:** weitere Busse und Verbindungen mit Umstieg oder ab einer anderen Haltestelle.
- **Karte:** Haltestellen an ihren echten Koordinaten, Linienverlauf aus den Fahrplandaten, berechnete Busposition. Karte minimierbar und als Vollbild nutzbar.
- **Fahrtverlauf:** alle Halte der Fahrt mit Zeiten und der aktuellen Busposition.
- Helles und dunkles Design, passend zur Systemeinstellung.

## Benutzung

1. App-Adresse öffnen: `https://<github-name>.github.io/<repository>/`
2. Auf dem iPhone in Safari **Teilen → Zum Home-Bildschirm** wählen. Die App startet dann wie eine normale App.
3. Beim ersten Start das **Zahnrad** öffnen:
   - eigene Haltestelle suchen und auswählen (die kleine Karte zeigt die Lage zur Kontrolle),
   - Ziele auswählen, z. B. „Bünde Bahnhof“ und „Bünde Marktplatz“,
   - Gehzeiten eintragen.
4. Solange keine Haltestelle eingerichtet ist, zeigt die App eine Vorschau mit gekennzeichneten Beispieldaten.

## Einrichtung mit GitHub Pages

1. `index.html` (und diese `README.md`) in ein **öffentliches** Repository hochladen.
2. **Settings → Pages**: bei „Source“ **Deploy from a branch**, bei „Branch“ **main** und **/ (root)** wählen und speichern.
3. Nach ein bis zwei Minuten ist die App unter `https://<github-name>.github.io/<repository>/` erreichbar.
4. Für ein Update einfach die neue `index.html` hochladen und die vorhandene ersetzen. Falls danach noch die alte Version erscheint, Safari einmal schließen und neu öffnen.

## Datenquellen

| Quelle | Wofür | Hinweis |
|---|---|---|
| [v6.db.transport.rest](https://v6.db.transport.rest/) | Echtzeitdaten der Deutschen Bahn (wie DB Navigator) | freie Community-API, zeitweise gestört |
| [Transitous](https://transitous.org) | Ausweichquelle, wenn die DB-Schnittstelle nicht antwortet | [Datenquellen von Transitous](https://transitous.org/sources/) |
| [OpenStreetMap](https://www.openstreetmap.org/copyright) | Kartenhintergrund | © OpenStreetMap-Mitwirkende |
| [Leaflet](https://leafletjs.com) | Kartendarstellung | über cdnjs geladen |

In den Einstellungen lässt sich wählen: **Automatisch** (DB, bei Störung Transitous), **Nur DB** oder **Nur Transitous**. Unter „Stand: …“ zeigt die App, welche Quelle gerade genutzt wird.

## Wie die Busposition berechnet wird

Die öffentlichen Schnittstellen liefern keine GPS-Position der Busse. Die App berechnet die Position deshalb aus den Echtzeit-Zeiten an den Halten:

1. Die Fahrt des Busses wird mit allen Halten, Zeiten und dem Linienverlauf geladen.
2. Jede Haltestelle wird auf den Linienverlauf projiziert.
3. Aus der aktuellen Uhrzeit ergibt sich, zwischen welchen zwei Halten der Bus ist. Die Position wird anteilig entlang der echten Strecke dazwischen eingezeichnet.

Die Anzeige ist also eine Schätzung auf Basis der Verspätungsdaten und keine Live-Ortung.

## Grenzen

- Einsatzwagen und Schulverstärker erscheinen nur, wenn das Verkehrsunternehmen sie in den Fahrplandaten veröffentlicht.
- Liefert keine Quelle Echtzeitdaten für eine Fahrt, zeigt die App die Fahrplanzeit und weist darauf hin.
- Die Schnittstellen haben Anfragegrenzen. Die App fragt deshalb nur beim Öffnen und auf Knopfdruck ab, nicht automatisch im Sekundentakt.

## Datenschutz

Die App speichert Haltestellen, Ziele und Gehzeiten nur lokal im Browser des jeweiligen Geräts (`localStorage`). Es gibt kein Konto und keinen eigenen Server. Beim Abrufen der Daten werden die Fahrplandienste und der Kartendienst direkt vom Browser aus angefragt.

## Lizenz

MIT-Lizenz, siehe [LICENSE](LICENSE).

Private, nicht-kommerzielle Familien-App. Die Daten von Transitous dürfen laut deren [Nutzungsbedingungen](https://transitous.org/api/) nur in nicht-kommerziellen Open-Source-Projekten verwendet werden.
