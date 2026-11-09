# Schulbus 646

Web-App für den Schulweg: Sie zeigt, wann man losgehen muss, um den Bus der Linie 646 von Hiddenhausen Richtung Bünde zu erreichen. Dazu kommen Echtzeit-Verspätungen, Alternativen, die berechnete Position des Busses und eine Fußweg-Navigation zur Haltestelle.

Die App besteht aus einer einzigen Datei (`index.html`). Sie läuft im Browser auf iPhone, iPad und Computer und braucht keinen eigenen Server, keine Anmeldung und keinen API-Schlüssel.

## Funktionen

**Abfahrt und Countdown**
- **Countdown bis zum Losgehen:** „Los in 4:30 · Losgehen um 07:04“, berechnet aus der Echtzeit-Abfahrt minus Gehzeit.
- **Gehzeiten Schnell / Normal / Langsam:** frei einstellbar, täglich per Umschalter wählbar.
- **Verspätungen und Ausfälle:** Abfrage beim Öffnen der App, beim Zurückkehren in die App und per Knopf „Verspätung jetzt prüfen“ in der Leiste am unteren Rand.
- **Ankunftszeiten** an den Zielen, z. B. „Bünde Marktplatz 07:31 · Bünde Bahnhof/ZOB 07:35“.

**Busse und Ziele**
- **Mehrere Ziele** (bis zu drei), z. B. Bünde Bahnhof und Bünde Marktplatz. Oben steht immer der früheste erreichbare Bus, der von der eigenen Haltestelle direkt zu einem Ziel fährt.
- **Einsatzwagen:** Fahrten mit Zusatzbuchstaben (z. B. E646) werden der Linie zugeordnet und gekennzeichnet. Andere Linien, die direkt zu einem Ziel fahren, tragen das Schild „andere Linie“.
- **Mehrere Linien** möglich, z. B. „646, 648“.
- **Alternativen:** weitere Busse sowie Verbindungen mit Umstieg oder ab einer anderen Haltestelle.

**Karte**
- Haltestellen an ihren echten Koordinaten, Linienverlauf aus den Fahrplandaten, berechnete Busposition.
- Karte minimierbar und als Vollbild nutzbar; der Kartenausschnitt bleibt beim Aktualisieren erhalten.
- **Fahrtverlauf** als Leiste mit allen Halten, Zeiten und der aktuellen Busposition.

**Fußweg-Navigation**
- Route vom aktuellen Standort zur Haltestelle als gestrichelte Linie auf der Karte.
- Abbiegehinweise („In 120 m · Links abbiegen · Rathausstraße“), Restweg, Gehzeit und Ankunft an der Haltestelle.
- Einschätzung, ob der Bus noch erreicht wird (grün / knapp / zu spät).
- Während der Navigation rechnet der Countdown mit dem echten Restweg statt mit der eingestellten Gehzeit. Bei Abweichung vom Weg wird neu berechnet, der Bildschirm bleibt an.

**Bedienung**
- Für Smartphones optimiert: große Schrift, große Tippflächen, wichtigste Infos oben, Aktualisieren-Knopf im Daumenbereich. Auf dem iPad und am Computer zweispaltig mit großer Karte.
- Helles und dunkles Design, passend zur Systemeinstellung.
- Eigenes Symbol für den Home-Bildschirm.

## Dateien im Repository

| Datei | Inhalt |
|---|---|
| `index.html` | die komplette App |
| `apple-touch-icon.png` | Symbol für den Home-Bildschirm |
| `README.md` | diese Beschreibung |
| `LICENSE` | MIT-Lizenz |

## Einrichtung mit GitHub Pages

1. Die Dateien in ein **öffentliches** Repository hochladen.
2. **Settings → Pages**: bei „Source“ **Deploy from a branch**, bei „Branch“ **main** und **/ (root)** wählen und speichern.
3. Nach ein bis zwei Minuten ist die App erreichbar unter `https://<github-name>.github.io/<repository>/`.
4. **Update:** neue `index.html` über **Add file → Upload files** hochladen; sie ersetzt die alte. Falls danach noch die alte Version erscheint, Safari einmal ganz schließen und neu öffnen.

## Benutzung

1. App-Adresse in Safari öffnen und über **Teilen → Zum Home-Bildschirm** als App ablegen.
2. Beim ersten Start das **Zahnrad** öffnen:
   - **Eure Haltestelle** aus der Liste der Linie wählen. Angeboten werden nur Halte, die die Linie wirklich anfährt, in Fahrtreihenfolge. Das Eingabefeld filtert die Liste, die kleine Karte zeigt die Lage zur Kontrolle.
   - **Ziele** wählen, z. B. „Bünde Bahnhof“ und „Bünde Marktplatz“. Liegt ein Ziel nicht an der Linie, sucht die App automatisch im Umkreis von 25 km.
   - **Gehzeiten** für Schnell, Normal und Langsam eintragen.
3. Morgens die App öffnen: Sie lädt sofort die aktuellen Abfahrten.
4. Für die Navigation **„Weg zur Haltestelle“** antippen und den Standortzugriff erlauben.

Solange keine Haltestelle eingerichtet ist, zeigt die App eine Vorschau mit gekennzeichneten Beispieldaten.

## Datenquellen

| Quelle | Wofür | Hinweis |
|---|---|---|
| [v6.db.transport.rest](https://v6.db.transport.rest/) | Echtzeitdaten der Deutschen Bahn (wie DB Navigator) | freie Community-API (Unterbau: db-vendo-client), zeitweise gestört |
| [Transitous](https://transitous.org) | Ausweichquelle, wenn die DB-Schnittstelle nicht antwortet | [Datenquellen von Transitous](https://transitous.org/sources/) |
| [FOSSGIS-Routing (OSRM)](https://routing.openstreetmap.de/about.html) | Fußwege für die Navigation | höchstens 1 Anfrage pro Sekunde; die App fragt nur beim Start und bei Abweichung vom Weg |
| [OpenStreetMap](https://www.openstreetmap.org/copyright) | Kartenhintergrund | © OpenStreetMap-Mitwirkende |
| [Leaflet](https://leafletjs.com) | Kartendarstellung | über cdnjs geladen |

In den Einstellungen lässt sich wählen: **Automatisch** (DB, bei Störung Transitous), **Nur DB** oder **Nur Transitous**. Ist die DB gestört, nutzt die App für 10 Minuten direkt Transitous. Unter „Stand: …“ steht, welche Quelle gerade genutzt wird.

### Haltestellen-IDs

Die App enthält keine fest eingetragenen Haltestellen-Nummern. Die DB und Transitous verwenden unterschiedliche IDs für dieselbe Haltestelle. Die App speichert deshalb Name und Koordinaten der gewählten Haltestelle und schlägt die passende ID der jeweiligen Quelle per Suche nach. Ein Treffer wird nur übernommen, wenn er höchstens 400 m von den gespeicherten Koordinaten entfernt ist.

Die Liste der Linienhalte entsteht aus echten Fahrten der Linie in beide Richtungen und wird eine Woche zwischengespeichert.

## Wie die Busposition berechnet wird

Die öffentlichen Schnittstellen liefern keine GPS-Position der Busse. Die App berechnet die Position aus den Echtzeit-Zeiten an den Halten:

1. Die Fahrt des Busses wird mit allen Halten, Zeiten und dem Linienverlauf geladen.
2. Jede Haltestelle wird auf den Linienverlauf projiziert.
3. Aus der aktuellen Uhrzeit ergibt sich, zwischen welchen zwei Halten der Bus ist. Die Position wird anteilig entlang der echten Strecke dazwischen eingezeichnet.

Die Anzeige ist eine Schätzung auf Basis der Verspätungsdaten und keine Live-Ortung.

## Grenzen

- Einsatzwagen und Schulverstärker erscheinen nur, wenn das Verkehrsunternehmen sie in den Fahrplandaten veröffentlicht.
- Pro Ziel prüft die App die nächsten Verbindungen ab „jetzt + Gehzeit“; das reicht etwa 30 bis 60 Minuten im Voraus.
- Liefert keine Quelle Echtzeitdaten für eine Fahrt, zeigt die App die Fahrplanzeit und weist darauf hin.
- Die Schnittstellen haben Anfragegrenzen. Die App fragt deshalb nur beim Öffnen und auf Knopfdruck ab; Countdown und Busposition laufen lokal weiter.
- Ist der Fußweg-Dienst nicht erreichbar, zeigt die Navigation die Luftlinie mit geschätzter Entfernung.

## Probleme lösen

| Meldung / Problem | Lösung |
|---|---|
| „… ist gerade nicht erreichbar“ | Die Datenquelle ist gestört. Mit „Automatisch“ wechselt die App selbst zu Transitous; sonst später erneut versuchen. |
| App bleibt bei Beispieldaten | Im Zahnrad eine Haltestelle auswählen und „Fertig“ tippen. |
| Standort wird nicht angeboten | iPhone: Einstellungen → Datenschutz & Sicherheit → Ortungsdienste → Safari-Websites → „Beim Verwenden der App“. |
| Alte Version wird angezeigt | Safari ganz schließen und neu öffnen. |
| Neues Symbol erscheint nicht | Altes Symbol vom Home-Bildschirm löschen und neu hinzufügen. |

## Datenschutz

Haltestellen, Ziele und Gehzeiten speichert die App nur lokal im Browser des jeweiligen Geräts (`localStorage`). Es gibt kein Konto und keinen eigenen Server. Fahrplandienste und Kartendienst werden direkt vom Browser aus angefragt.

Der Standort wird nur während der Fußweg-Navigation verwendet und von der App nicht gespeichert. Für die Routenberechnung werden Start- und Zielkoordinaten an das FOSSGIS-Routing übertragen, das Anfragen laut eigener Angabe protokolliert.

## Lizenz

MIT-Lizenz, siehe [LICENSE](LICENSE).

Private, nicht-kommerzielle Familien-App. Die Daten von Transitous dürfen laut deren [Nutzungsbedingungen](https://transitous.org/api/) nur in nicht-kommerziellen Open-Source-Projekten verwendet werden.
