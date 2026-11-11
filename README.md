# Aufgabenplaner (Monatsplaner & Aufgabenverteiler)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung für alle deutschen Bundesländer (Standard: Sachsen).

## Nutzung

Beim **automatischen Termine generieren** wird geprüft, ob zur gleichen Zeit (gleiches Datum + gleiche Uhrzeit, unabhängig von Zeitslot-ID und Ort) bereits ein Termin existiert – auch in überlappenden Kalenderwochen beim Monatswechsel und innerhalb eines Durchlaufs. So entstehen keine Doppel-Termine.

Im Monats-Kalender genügt ein **Tipp/Antippen auf den Tag**, um ihn auszuwählen – rechts im Aufgaben-Panel liegt dann die Schaltfläche **„+ Aufgabe anlegen“** für diesen Tag (ein extra „+ Aufgabe“-Button in jeder Zelle gibt es nicht mehr). Jede Aufgabe zeigt den zugeteilten Namen als **farbigen Chip** – jede Person hat eine feste Farbe, Zuteilungen sind so auf einen Blick erkennbar (mobil und am Desktop). Auf schmalen mobilen Zellen wird im Chip statt des Namens nur das **Akronym (Anfangsbuchstaben)** angezeigt, darunter klein die Uhrzeit – nichts wird mehr mit „…“ abgeschnitten; der volle Name steht im Tooltip bzw. nach dem Antippen des Tags im Aufgaben-Panel. Unbesetzte Aufgaben erscheinen rot („Offen"). Die Kalenderzellen **wachsen automatisch mit**, wenn an einem Tag mehrere Termine zusammenkommen – in den Zellen muss nicht mehr gescrollt werden.

Im Tab **⚙️ Stammdaten & Export** lässt sich das **Bundesland für die Feiertage** wählen – der Kalender zeigt dann die dortigen gesetzlichen Feiertage (inkl. der länderspezifischen wie Fronleichnam, Allerheiligen, Weltkindertag o. Ä.). Die Einstellung wird pro Gerät gespeichert.

## Gruppen

Im Tab **⚙️ Stammdaten & Export** lassen sich **Gruppen** anlegen (z. B. Teams oder Schichtgruppen) und Personen per Klick zuordnen. Einzelnen Aufgaben und wiederkehrenden Mustern können optional **relevante Gruppen** zugewiesen werden (leer = für alle relevant):

- Wiederkehrende Gruppen-Termine werden beim **Termine generieren** automatisch angelegt – auch parallel zu einem normalen Termin zur gleichen Zeit –, bekommen aber **keine automatische Zuteilung** (manuelle Vergabe).
- **Gruppen sind nachträglich änderbar**: Im Aufgaben-Panel „✏️ Ändern“ lässt sich die Gruppen-Relevanz jedes Termins jederzeit anpassen.
- Zuteilungen auf Gruppen-Terminen laufen **außer Konkurrenz**: Sie haben keinen Einfluss auf die Fairness-Verteilung anderer Termine oder auf die manuelle Empfehlung.
- Muster- und Gruppen-Änderungen werden per **Last-Update-wins** synchronisiert und setzen sich damit zuverlässig auf dem anderen Gerät durch.
- - Gruppen-Termine erhalten beim Generieren eine **eigene ID mit Gruppen-Scope** – ein Gruppentermin kann dadurch nicht mehr versehentlich mit einem Normaltermin gleichen Zeitslots/Ortes verschmolzen oder beim Dedupe verworfen werden.
- Bei der **automatischen Zuteilung** werden Aufgaben mit Gruppen nur an Mitglieder dieser Gruppen vergeben.
- In der **Verfügbarkeits-Liste** werden Nicht-Mitglieder ausgegraut mit dem Grund „Nicht in relevanter Gruppe“.
- Bei Terminen mit **mehreren Gruppen** sind Mitglieder **aller** ausgewählten Gruppen wählbar – bei Gruppenterminen entscheidet die Gruppen-Zugehörigkeit, die Qualifikations-Matrix blockiert nicht zusätzlich (Abwesenheiten und Schicht-Blockaden gelten weiterhin).
- Im Kalender tragen Gruppen-Termine ein kleines **Indigo-Badge mit dem Gruppen-Kürzel** am Termin-Chip; mehrere Termine pro Tag sind durch die vorangestellte Uhrzeit klar getrennt.
- Wird eine Gruppe gelöscht, wird sie automatisch aus allen Terminen und Mustern entfernt.

## Monatliche Muster

Wiederkehrende Muster können neben „Wöchentlich“ und „Alle 2 Wochen“ auch **monatlich** laufen:

- **Am gleichen Tag des Monats** (z. B. jeden 15.) – Monate ohne diesen Tag werden übersprungen.
- **Am x. Wochentag im Monat** (z. B. am 2. Montag oder am letzten Freitag). Wochenwerte werden als Zahl gespeichert und beim Prüfen tolerant interpretiert – bestehende Muster mit String-Werten (z. B. „1“ aus dem Auswahlfeld) funktionieren ebenfalls zuverlässig.

Monatliche Termine werden unabhängig vom Wochentag des Zeitslots erzeugt; die Uhrzeit kommt weiterhin vom Zeitslot.

Einfach `index.html` in einem Browser öffnen (Internetverbindung nötig, da React/Tailwind per CDN geladen werden). Alle Daten bleiben lokal im Browser (localStorage).

## Faire automatische Zuteilung

Die automatische Zuteilung („2. Personen zuteilen") und die Empfehlung im Aufgaben-Panel verwenden dieselbe Fairness-Bewertung. Pro Aufgabe wird unter allen **verfügbaren** Personen gewählt nach:

1. **Längster Pause** – wer am längsten keinen Einsatz hatte, kommt zuerst (noch nie Eingesetzte zuerst),
2. **weniger Gesamteinsätze**,
3. **Wochentags-Balance** – wer an diesem Wochentag seltener dran war,
4. alphabetisch (entscheidet nur bei völliger Gleichheit).

Die Bewertung zählt bei der **automatischen Zuteilung** nur bereits vergangene oder gleichzeitige Einsätze – automatisch zugeteilte Zukunftstermine fließen nicht ein, damit die Verteilung auch dann fair bleibt, wenn weit im Voraus Termine generiert wurden. **Manuell eingeteilte Termine zählen jedoch auch in der Zukunft**, ebenso bei der **Empfehlung**, die alle zukünftigen Termine berücksichtigt. Die Pause wird dabei nie negativ – ein Zukunftstermin verschlechtert die eigene Bilanz, ohne die Verteilung kippen zu lassen. Damit wird niemand übersprungen, wenn andere verfügbar sind. Bereits zugeteilte Aufgaben bleiben beim erneuten Ausführen unverändert; mit **„🧹 Zuteilungen zurücksetzen (Monat)"** und erneutem „Personen zuteilen" wird ein Monat komplett neu und fair verteilt.

Im Aufgaben-Panel ist die **Verfügbarkeits-Liste direkt bedienbar**: Jeder Name ist ein Button – Klick weist die Person zu (erneuter Klick auf die zugewiesene Person gibt die Zuteilung wieder frei). Die ⭐-Empfehlung ist ebenfalls anklickbar; gesperrte Personen sind ausgegraut und zeigen den **Grund** (z. B. Schicht, Abwesenheit, nicht qualifiziert).

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

**Löschungen:** Gelöschte Einträge werden als **Lösch-Markierung (Tombstone) mit Zeitstempel** gemerkt und mit synchronisiert – so werden gelöschte Orte, Zeitslots, Muster, Personen, Abwesenheiten und Termine auf dem anderen Gerät ebenfalls entfernt und nicht durch den Abgleich wiederhergestellt.

**Zusammenführung:** Beide Datensätze werden vereinigt (union by id) – neue Einträge vom anderen Gerät werden übernommen. **Termine (inkl. Zuteilungen) werden nach dem Last-Update-wins-Prinzip abgeglichen:** Die jeweils zuletzt gemachte Änderung (neuerer Zeitstempel) setzt sich auf beiden Geräten durch. Wird ein gelöschter Termin danach neu angelegt oder geändert (z. B. durch erneutes Termine generieren), belebt er die Lösch-Markierung wieder. Bei Personen werden Abwesenheiten vereinigt, Matrix-Häkchen kombiniert (aktiv gewinnt) und der Schichtrhythmus übernommen, falls lokal keiner gesetzt ist.

**Verlässlichkeit:** Der Empfang ist idempotent – mehrfach übertragene identische Daten ändern nichts und erzeugen keine Protokoll-Einträge. Bricht die Datenverbindung ab, wird sie automatisch (alle paar Sekunden) neu aufgebaut, ohne dass die Kopplung verloren geht. Bei einem **Versionsunterschied** zwischen den Geräten erscheint eine klare Warnung im Sync-Protokoll.

**Wichtig:** Damit Änderungen korrekt abgeglichen werden, sollte auf **beiden Geräten die gleiche App-Version** (mindestens 1.8.1) laufen.

