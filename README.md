[README (1).md](https://github.com/user-attachments/files/35009499/README.1.md)
# 🛴 ScoutRoute

**Zufällige Rundrouten für E-Scooter-Fahrer, mit Supermarkt-Stopp fürs Rideout.**

ScoutRoute ist eine kostenlose Android-App. Du stellst die Reichweite deines Scooters ein, die App plant eine Rundstrecke von deinem Standort aus und führt sie meistens an einem Supermarkt vorbei, damit du unterwegs noch Snacks und Getränke holen kannst. Keine Lust auf die Route? Ein Klick auf **Change Route** und es gibt eine komplett andere.

> **Status:** in Entwicklung (frühe Version). Karte, Routenplanung und Routenverlauf funktionieren, die Live-Navigation mit Abbiegehinweisen kommt als Nächstes.

---

## Was kann die App?

- **Zufällige Rundrouten:** Start und Ziel sind dein aktueller Standort, die Strecke ist jedes Mal anders
- **Reichweitenplanung:** Die Tour wird so geplant, dass der Rückweg sicher drin ist. Geplant werden etwa 60 % der eingestellten Reichweite (maximal 75 %), als Puffer für Steigungen, Wind und Kälte
- **Supermarkt-Stopp:** Auf der Route liegt ein Supermarkt aus den OpenStreetMap-Daten. Kürzlich besuchte Märkte werden möglichst nicht wiederholt
- **Change Route:** Neue zufällige Route mit einem Klick
- **Routenverlauf:** Gestartete Routen werden nur auf deinem Handy gespeichert. Neue Routen, die den letzten zu ähnlich sind, werden aussortiert, damit nicht jeden Tag dieselbe Strecke kommt
- **Abkürzungen bevorzugen:** Optional wählt die Routenplanung die kürzeste Strecke statt der empfohlenen. Das führt öfter über kleine Verbindungswege, die für Fahrräder freigegeben sind
- **Karte und GPS:** OpenStreetMap-Karte mit deinem Live-Standort, die Route wird als blaue Linie angezeigt

---

## 📲 Installation (Android)

Die App gibt es **nicht im Google Play Store**, du installierst sie direkt als APK-Datei.

1. Öffne auf deinem Handy die Seite **Releases** dieses Repositorys und lade die neueste `.apk`-Datei herunter.
2. Tippe die heruntergeladene Datei an. Android fragt, ob dein Browser (oder die Dateien-App) Apps installieren darf. Erlaube das für diese eine Installation. Du findest die Einstellung auch unter *Einstellungen → Apps → Besondere App-Zugriffe → Unbekannte Apps installieren*.
3. Eventuell zeigt **Google Play Protect** eine Warnung wie "App nicht bekannt" oder "App scannen". Wähle **Mehr Details → Trotzdem installieren** (Hintergrund siehe unten).
4. Öffne die App. Beim ersten Start fragt sie nach dem **Standort**. Wähle *Beim Verwenden der App* und *Genau*, sonst funktioniert die Routenplanung nicht.
5. Wenn du magst, kannst du danach die Erlaubnis "Unbekannte Apps installieren" für deinen Browser wieder ausschalten.

Zum Entfernen: App wie jede andere deinstallieren, dann sind auch der gespeicherte Verlauf und alle Einstellungen weg.

### So bedienst du die App

1. Warte kurz, bis dein Standort als Punkt auf der Karte erscheint (draußen geht das schneller).
2. Stelle mit dem Schieberegler deine **Reichweite** ein.
3. Schalte **Abkürzungen bevorzugen** nach Wunsch ein oder aus.
4. Tippe auf **Route generieren**. Nach ein paar Sekunden erscheint die Route mit dem Supermarkt als Marker, oben im Panel stehen Länge und Name des Marktes.
5. Gefällt sie dir nicht, tippe auf **Change Route**.
6. Mit **Start** speicherst du die Route im Verlauf.
7. Der Knopf **📍** oben rechts springt zurück zu deinem Standort.

Falls die App beim ersten Start nach einem **OpenRouteService-Key** fragt (siehe unten), trägst du ihn dort einmal ein. Er wird nur auf deinem Handy gespeichert. Über das **⚙** oben links kannst du ihn später ändern.

---

## 🙂 Keine Angst vor der Warnung

Android warnt bei Apps, die nicht aus dem Play Store kommen, ganz allgemein. Das liegt nicht daran, dass mit dieser App etwas nicht stimmt, sondern daran, dass sie weder im Play Store veröffentlicht noch mit einem offiziellen Store-Zertifikat signiert ist. Sie trägt nur ein einfaches Test-Zertifikat, wie es Android Studio beim Entwickeln erzeugt. Eine Prüfung durch Google hat nicht stattgefunden, deshalb die Rückfrage.

Damit du selbst einschätzen kannst, ob du ihr vertraust:

- **Der gesamte Quellcode ist offen.** Du kannst ihn hier lesen oder die App sogar selbst bauen (siehe unten), dann musst du keiner fertigen APK vertrauen.
- **Wenige Berechtigungen:** Standort (genau und ungefähr), Internet und Netzwerkstatus. Keine Kontakte, kein Mikrofon, keine Kamera, keine Fotos, keine SMS.
- **Kein Konto, keine Werbung, keine Tracker, keine Analyse-Dienste.**
- **Dein Verlauf bleibt auf dem Handy.** Er liegt als Datei im App-Speicher und wird nirgendwohin hochgeladen.

**Was die App ins Internet schickt (ehrlich gesagt):** Damit sie funktioniert, muss sie Koordinaten an Dienste senden. Das sind Anfragen an die Supermarkt-Suche (Overpass-API), den Routenplaner (OpenRouteService, eventuell über einen Zwischenserver des Entwicklers) und die Kartenserver. Diese Dienste sehen daher ungefähr, wo du dich befindest, genau wie bei jeder anderen Karten-App. Wenn dir das nicht recht ist, installiere die App besser nicht.

**Prüfsumme (optional):** Steht im Release eine SHA-256-Prüfsumme, kannst du die heruntergeladene Datei damit vergleichen. Unter Windows in der PowerShell: `Get-FileHash app-debug.apk`. Stimmt der Wert, ist die Datei unverändert.

Wenn du unsicher bist: nicht installieren. Das ist völlig in Ordnung.

---

## 🛠️ Selbst bauen

1. Projekt in **Android Studio** öffnen (Min. SDK 26).
2. Die Datei `AppConfig.kt.example` kopieren und als `AppConfig.kt` im selben Ordner (`app/src/main/java/com/example/rideout/`) ablegen. Dort bei `USER_AGENT` einen eigenen App-Namen mit Kontaktadresse eintragen. Das ist wichtig, weil die OpenStreetMap-Kartenserver Apps mit Platzhalter-Namen sperren.
3. **Routing:** Hole dir einen kostenlosen Key bei [openrouteservice.org](https://openrouteservice.org) (*Dashboard → Request a token*) und trage ihn bei `ORS_KEY` ein oder lass das Feld leer, dann fragt die App beim ersten Start danach.
   Wenn du die App an andere weitergeben willst, ohne dass diese einen Key brauchen, richte mit `worker.js` einen kostenlosen Zwischenserver bei Cloudflare ein und fülle `PROXY_URL` und `PROXY_TOKEN` aus.
4. Handy per USB anschließen (USB-Debugging aktivieren) und die App starten, oder über *Build → Build APK(s)* eine APK erzeugen.

Die echte `AppConfig.kt` ist absichtlich nicht im Repository, damit niemand fremde Keys oder Adressen veröffentlicht.

---

## 🧰 Technik

| Bereich | Eingesetzt |
|---|---|
| Sprache und Oberfläche | Kotlin, Jetpack Compose (Material 3) |
| Karte | [osmdroid](https://github.com/osmdroid/osmdroid) mit OpenStreetMap-Kacheln |
| Standort | Google Play Services Location (Fused Location Provider) |
| Routing | [OpenRouteService](https://openrouteservice.org) (Profil `cycling-regular`) |
| Supermärkte | [Overpass API](https://overpass-api.de) (OpenStreetMap-Daten) |
| Speicher | Routenverlauf als lokale JSON-Datei |
| Zwischenserver (optional) | Cloudflare Worker (`worker.js`) |

---

## ⚠️ Wichtige Hinweise

- **Verkehrsregeln:** Die App nutzt ein **Fahrradprofil** als Annäherung, ein spezielles E-Scooter-Routing gibt es nicht. Die Routen sind nur Vorschläge, **achte unterwegs immer auf Beschilderung und Verkehrsregeln**. In Deutschland dürfen E-Scooter nicht auf Gehwegen fahren, deshalb werden diese nicht gezielt eingeplant. Die Option "Abkürzungen" wählt lediglich kürzere Wege, die für Fahrräder freigegeben sind. Ob ein Weg für E-Scooter erlaubt ist, kann die App nicht wissen.
- **Reichweite:** Der Akkuverbrauch hängt von Gewicht, Steigungen, Temperatur und Fahrweise ab. Die Planung ist eine Schätzung, plane genug Puffer ein.
- **Keine Garantie:** Die App ist ein Hobbyprojekt und wird ohne Gewähr bereitgestellt. Die Nutzung erfolgt auf eigene Verantwortung.
- **Kontingente:** Die kostenlosen Dienste (Routing, Karte, Supermarktsuche) haben Nutzungsgrenzen. Wenn zu viele Leute sie gleichzeitig nutzen, kann es zu Fehlermeldungen kommen. Probier es dann später nochmal.

---

## 🗺️ Geplant

- Live-Navigation in der App mit Abbiegehinweisen und Sprachausgabe
- Automatische Neuberechnung, wenn man von der Route abkommt
- Verlauf in der Oberfläche anzeigen und alte Routen erneut fahren
- Eigener Kartenanbieter für viele Nutzer

---

## Quellen und Lizenzen

Kartendaten © OpenStreetMap-Mitwirkende (ODbL). Routing durch OpenRouteService, Supermarktdaten über die Overpass API.
