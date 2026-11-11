# Aufgabenplaner (Monatsplaner & Aufgabenverteiler – Sachsen)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung (Sachsen).

## Nutzung

Einfach `index.html` in einem Browser öffnen (Internetverbindung nötig, da React/Tailwind per CDN geladen werden). Alle Daten bleiben lokal im Browser (localStorage).

## Schichtarbeit (externes 3-Schicht-System)

Mitarbeiter im externen 3-Schicht-System können pro Person hinterlegt werden: In **Personen & Matrix** unter „Externes Schichtsystem (Rotation)" den Modus „Arbeitet extern im Schichtsystem" wählen.

Dazu gehören:

- **Startdatum**: Der Tag, an dem der Rhythmus beginnt (Tag 1 des Musters).
- **Rhythmus**: Beliebig lange Abfolge von Schicht-Tagen als farbige Kacheln – F = Frühschicht, S = Spätschicht, N = Nachtschicht, – = frei. Kacheln antippen zum Durchschalten, mit +/− die Musterlänge ändern. Standard ist ein 8-Tage-Rhythmus (F F S S N N – –), der sich am Startdatum wiederholt.
- **Vorschau**: Die nächsten 7 Tage zeigen sofort, welche Schicht an welchem Tag greift.

Daraus ergibt sich automatisch, wann jemand für Aufgaben zur Verfügung steht:

- **Frühschicht** (06–14 Uhr): vormittags blockiert, nachmittags verfügbar
- **Spätschicht** (14–22 Uhr): nachmittags blockiert, vormittags verfügbar
- **Nachtschicht** (22–06 Uhr): abends/nachts blockiert **und** am Folgetag bis 14 Uhr (keine Aufgaben am nächsten Morgen)

Die automatische Zuteilung, die Empfehlungen und die manuelle Auswahl berücksichtigen diese Blockierungen (nicht verfügbare Personen werden mit der Begründung „Schicht: …" gekennzeichnet). Vor dem Startdatum besteht keine Einschränkung.

## PWA installation & Versionshinweis

Die Seite ist als **PWA (Progressive Web App)** installierbar – Voraussetzung ist, dass sie über HTTPS bereitgestellt wird (z. B. GitHub Pages). Dann erscheint im Browser die Option „Installieren“/„Zum Home-Bildschirm“. Dazu gehören `manifest.webmanifest`, `icons/` und der Service Worker `sw.js`. Beim lokalen Öffnen als Datei (`file://`) ist die Installation nicht verfügbar, die App funktioniert aber unverändert.

Im Header wird die aktuelle Versionsnummer (z. B. **v1.1.0**) angezeigt. Die App prüft beim Laden, alle 5 Minuten und beim Fensterfokus per `version.json`, ob auf dem Server eine neuere Version liegt. Falls ja, erscheint oben ein gelber Hinweis mit Button **„Jetzt aktualisieren“** (lädt die Seite neu).

Beim Aktualisieren einer neuen Version: `APP_VERSION` in `index.html` **und** `version.json` auf denselben neuen Wert setzen.

## Geräte-Synchronisation (WebRTC Data Channels)

Über den Button **🔄 Geräte-Sync** können zwei Geräte direkt peer-to-peer synchronisiert werden:

1. **Gerät 1:** „Sync starten“ erzeugt einen 8-stelligen Sync-Code – als QR-Code **und** als Text mit Kopieren-Button.
2. **Gerät 2:** „Mit Code verbinden“ – Code eingeben (oder einfügen) und verbinden.
3. Verbunden: Orte, Zeitslots, Muster, Personen, Abwesenheiten und Termine werden automatisch in beide Richtungen übertragen.

Der Handshake läuft über das öffentliche PeerJS-Signaling; die Plandaten selbst gehen direkt Gerät-zu-Gerät (peer-to-peer) und werden auf keinem Server gespeichert.
