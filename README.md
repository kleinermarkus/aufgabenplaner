# Aufgabenplaner (Monatsplaner & Aufgabenverteiler – Sachsen)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung (Sachsen).

## Nutzung

Einfach `index.html` in einem Browser öffnen (Internetverbindung nötig, da React/Tailwind per CDN geladen werden). Alle Daten bleiben lokal im Browser (localStorage).

## Faire automatische Zuteilung

Die automatische Zuteilung („2. Personen zuteilen") und die Empfehlung im Aufgaben-Panel verwenden dieselbe Fairness-Bewertung. Pro Aufgabe wird unter allen **verfügbaren** Personen gewählt nach:

1. **Längster Pause** – wer am längsten keinen Einsatz hatte, kommt zuerst (noch nie Eingesetzte zuerst),
2. **weniger Gesamteinsätze**,
3. **Wochentags-Balance** – wer an diesem Wochentag seltener dran war,
4. alphabetisch (entscheidet nur bei völliger Gleichheit).

Damit wird niemand übersprungen, wenn andere verfügbar sind, und die Empfehlung entspricht der automatischen Zuteilung. Bereits zugeteilte Aufgaben bleiben beim erneuten Ausführen unverändert; mit **„🧹 Zuteilungen zurücksetzen (Monat)"** und erneutem „Personen zuteilen" wird ein Monat komplett neu und fair verteilt.

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

Über den Button **🔄 Geräte-Sync** im Kopf können zwei Geräte direkt peer-to-peer synchronisiert werden:

1. **Gerät 1:** „Sync starten“ erzeugt einen 8-stelligen Sync-Code – angezeigt als **QR-Code** und als **Text mit Kopieren-Button** (📋 Code kopieren).
2. **Gerät 2:** „Mit Code verbinden“ – den Code ins Textfeld **eingeben oder per Zwischenablage einfügen** (📋 Code aus Zwischenablage einfügen) und verbinden.
3. **Automatische Live-Synchronisierung:** Nach dem ersten Abgleich werden alle Änderungen automatisch in beide Richtungen übertragen – solange beide Geräte die App geöffnet haben.

### Dauerhafte Kopplung

Die Sync-Verbindung wird **dauerhaft gespeichert**:

- Beim nächsten Öffnen der App verbindet sich das Gerät **automatisch wieder** mit dem gekoppelten Gerät – kein erneuter Code-Austausch.
- Der **Sync-Code des wartenden Geräts bleibt erhalten**: Wird der Sync erneut gestartet, wird der bisherige Code weiterverwendet – das andere Gerät kann sich also weiterhin mit dem bekannten Code verbinden.
- Bricht die Verbindung ab (z. B. Netzwerkwechsel, das andere Gerät ist offline), wird **automatisch alle 10 Sekunden** erneut verbunden, bis es wieder klappt. Nach Netzwerk-Rückkehr oder App-Aktivierung (z. B. nach dem Schlafmodus) wird sofort neu verbunden.
- Für Verbindungen über Mobilfunk oder strenge Firewalls werden zusätzlich **TURN-Relay-Server** verwendet, wenn eine Direktverbindung nicht möglich ist.
- Status im Kopf: **🟢 Sync aktiv** (verbunden), **🟡 Sync verbindet…** (Wiederverbindungsversuch läuft) oder **🔄 Geräte-Sync** (nicht gekoppelt).
- Die Kopplung lösen: **🔌 Verbindung trennen (Kopplung löschen)** im Sync-Dialog.

### Sync-Protokoll

Im Hintergrund wird ein **Protokoll** geführt (persistiert im Browser, max. 200 Einträge): Verbindungsstatus, Wiederverbindungsversuche, gesendete (↑) und empfangene (↓) Übertragungen mit Zeitstempel sowie Fehler. Einsehbar im Geräte-Sync-Dialog unten unter **📜 Sync-Protokoll**, dort auch per Button löschbar.

Der Handshake läuft über das öffentliche PeerJS-Signaling; die Plandaten selbst gehen direkt Gerät-zu-Gerät (peer-to-peer) und werden auf keinem Server gespeichert. Hintergrund-Synchronisierung setzt voraus, dass die App auf beiden Geräten geöffnet (bzw. als PWA installiert) ist.

**Löschungen:** Gelöschte Einträge werden als **Lösch-Markierung (Tombstone)** gemerkt und mit synchronisiert – so werden gelöschte Orte, Zeitslots, Muster, Personen, Abwesenheiten und Termine auf dem anderen Gerät ebenfalls entfernt und nicht durch den Abgleich wiederhergestellt.

**Zusammenführung:** Beide Datensätze werden vereinigt (union by id) – bestehende lokale Einträge bleiben erhalten, neue Einträge vom anderen Gerät werden übernommen. Bei Personen werden Abwesenheiten vereinigt, Matrix-Häkchen kombiniert (aktiv gewinnt) und der Schichtrhythmus übernommen, falls lokal keiner gesetzt ist.

