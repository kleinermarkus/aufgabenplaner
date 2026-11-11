# Aufgabenplaner (Monatsplaner & Aufgabenverteiler)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung für alle deutschen Bundesländer (Standard: Sachsen).

## Funktionsübersicht

### 📅 Planer & Kalender
- **Monatskalender** mit Feiertagen (Bundesland einstellbar), farbigen Namens-Chips (mobil als Akronym) und mitwachsenden Zellen bei mehreren Terminen pro Tag; Gruppentermine ohne Zuteilung zeigen statt „Offen“ den Gruppennamen (mobil das Gruppen-Kürzel, indigo-farben)
- **Tag antippen** → Aufgaben-Panel rechts: „+ Aufgabe anlegen", Zeitslot/Ort ändern, **Gruppen-Chips direkt am Termin umschalten**, ⭐ **Empfehlung** anklickbar, Verfügbarkeits-Liste mit Klick-Zuteilung und **Grundangaben** bei Sperren (Abwesend, Schicht, Qualifikation, Gruppe)
- **📅 Datum ändern** – jeden geplanten Termin direkt im Panel verschieben (Datumsfeld am Termin); der Kalender springt zum neuen Tag, Kollisionen zur gleichen Zeit werden abgefangen. Bei Wechsel des Wochentags fragt ein Banner, ob ein vorhandener Zeitslot des neuen Wochentags genutzt (mit Anzahl qualifizierter Personen) oder ein neuer Zeitslot angelegt werden soll – so bleiben die Qualifikationsprüfungen korrekt; ist die aktuell zugeteilte Person für den neuen Zeitslot nicht qualifiziert, gibt es einen Warnhinweis
- **1. Termine generieren (vollst. KW)** – legt alle wiederkehrenden Termine des Monats an (inkl. Gruppentermine), mit Duplikatschutz auch über Monatswechsel hinweg
- **2. Personen zuteilen** – faire automatische Verteilung (Gruppentermine ausgenommen)
- **🧹 Zuteilungen zurücksetzen (Monat)** – alle Zuteilungen des Monats freigeben, dann neu verteilen
- **🖨️ Monatsplan als PDF drucken** – druckoptimierte Ansicht
- **Monatstermine löschen** – alle Termine des angezeigten Monats entfernen

### 👥 Personen & Matrix
- **Personen anlegen/verwalten** (Name, Farbe automatisch)
- **Qualifikations-Matrix**: Pro Person × Zeitslot × Ort, wer kann was übernehmen
- **Abwesenheiten**: Pro Person manuell (Start/Ende, Grund) oder **Import aus PDF/DOCX**
- **Externes Schichtsystem (Rotation)**: Startdatum + beliebiger Rhythmus aus Früh/Spät/Nacht/frei (F/S/N/–), Vorschau der nächsten 7 Tage; Schichten blockieren automatisch (Früh 06–14, Spät 14–22, Nacht 22–06 + Folgetag bis 14 Uhr)

### 🔄 Wiederkehrende Muster
- **Wöchentlich**, **alle 2 Wochen** (Kalenderwochen-Parität) oder **monatlich** (fester Tag des Monats oder x. Wochentag im Monat, auch „letzter")
- **Optionale Gruppen-Relevanz** je Muster; Muster nachträglich bearbeiten/löschen

### 📊 Statistik
- Einsätze je Person **gesamt und je Wochentag** über den gesamten Plan

### ⚙️ Stammdaten & Export
- **Orte** und **Zeitslots** verwalten (Wochentag + Uhrzeit)
- **Gruppen verwalten**: Anlegen, Mitglieder zuordnen; Löschen entfernt die Gruppe aus allen Terminen/Mustern
- **Bundesland** für die gesetzlichen Feiertage (16 Länder, Standard Sachsen)
- **Datensicherung**: Backup als JSON exportieren/importieren, **gesamte App als .html herunterladen**
- **Gefahrenzone**: Alle Anwendungsdaten komplett löschen

### 🔄 Geräte-Sync (WebRTC)
- Kopplung zweier Geräte per **QR-Code oder Schlüssel-Text** (`Code.Token`) mit **Authentifizierungs-Handshake**
- **Automatischer Live-Sync** in beide Richtungen, **dauerhafte Kopplung** mit Auto-Wiederverbindung, **Sync-Protokoll** einsehbar
- **Löschungen werden mit synchronisiert** (Tombstones); Änderungen per Last-Update-wins; Versionswarnung bei unterschiedlichen App-Ständen

### 📱 PWA
- **Installierbar** als App; **Update-Banner** bei neuer Server-Version; **Versionsnummer** im Kopf
- **Offline-fähig:** Alle Bibliotheken werden lokal mitgeliefert und vom Service Worker vorgehalten – die App startet und arbeitet auch ohne Internetverbindung (nur die Geräte-Synchronisierung braucht dann Internet)

### 🛠️ Zentrale Programm-Funktionen (Code-Referenz)
| Funktion | Aufgabe |
|---|---|
| `generateDates` | Erzeugt Termine aus Mustern (volle Kalenderwochen, Duplikatschutz, Gruppen-IDs mit Gruppen-Scope) |
| `patternMatchesDate` / `isNthWeekdayOfMonth` | Prüft, ob ein Muster an einem Datum greift (wöchentlich/2-wöchentlich/monatlich) |
| `assignPeople` | Faire automatische Zuteilung (überspringt Gruppentermine) |
| `buildAssignmentStats` / `sortCandidatesByFairness` | Einsatz-Bilanz (Pause, Anzahl, Wochentags-Balance) und Sortierung |
| `getPersonAvailabilityDetails` | Verfügbarkeit + Grund (Gruppe, Abwesenheit, Schicht, Qualifikation) |
| `getExternalShiftBlock` | Schicht-Blockierung aus der Rotation pro Person |
| `getGermanHolidays` / `GERMAN_STATES` | Feiertage je Bundesland |
| `collectSyncData` / `applySyncData` | Sync-Payload bauen / empfangen (idempotent, Tombstones, LWW via `mergeTasks`) |
| `isHelloValid` / `isSyncEnvelope` / `generateSyncToken` | Sync-Handshake: Authentifizierung und Nachrichten-Umschläge |
| `getPairing` / `establishSync` / `scheduleReconnect` | Dauerhafte Kopplung, Verbindungsaufbau, Auto-Reconnect |

**Erstinstallation:** Beim allerersten Start (leerer Browser-Speicher) legt die App automatisch **anonyme Demo-Daten** an – 6 Beispielpersonen mit Qualifikations-Matrix, 2 Gruppen, Beispielmuster aller Art (wöchentlich, 2-wöchentlich, monatlich, 1. Wochentag im Monat, Gruppen-Muster), eine Beispiel-Abwesenheit, ein 3-Schicht-Beispiel und einige Termine im aktuellen Monat. Bereits genutzte Apps werden **nie überschrieben** – sobald eigene Daten vorhanden sind, greift keine Demo mehr. Wer eine Demo-App mit einer bereits genutzten App **koppelt**, entfernt die Demo-Daten beim ersten Datenabgleich **automatisch** (mit Protokoll-Eintrag „🧹 Demo-Daten automatisch entfernt“); Demo-Daten werden auch nie auf das andere Gerät übertragen. Zum Entfernen einfach die Demo-Einträge löschen (oder in der Gefahrenzone alle Daten zurücksetzen).

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

1. **Gerät 1:** „Sync starten“ erzeugt einen 8-stelligen Sync-Code **plus Geheimtoken** – angezeigt als **QR-Code** und als **Text mit Kopieren-Button** (📋 Schlüssel kopieren).
2. **Gerät 2:** „Mit Code verbinden“ – den vollständigen Schlüssel (`Code.Token`) ins Textfeld **eingeben oder per Zwischenablage einfügen** (📋 Schlüssel aus Zwischenablage einfügen) und verbinden.
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

### Sicherheit

- **Authentifizierungs-Handshake:** Der Schlüssel besteht aus dem sichtbaren Code **und einem geheimen Token** (QR-Code/Text enthalten beides). Vor dem ersten Datenabgleich prüfen beide Geräte das Token – Verbindungsversuche mit ungültigem Schlüssel werden abgewiesen und im Protokoll vermerkt. Ohne gültigen Schlüssel werden keine Daten übertragen.
- **Schlüssel wie ein Passwort behandeln:** Nur mit dem eigenen zweiten Gerät teilen. Bei Verdacht auf Missbrauch auf beiden Geräten „Kopplung löschen“ und neu koppeln (das erzeugt ein neues Token).
- **Integrität der Bibliotheken (SRI):** Alle Bibliotheken (React, ReactDOM, Tailwind, Mammoth, PDF.js inkl. Worker, PeerJS, QRCode) liegen versioniert im Repository und sind mit Subresource-Integrity-Hashes abgesichert – manipulierte oder veränderte Dateien blockiert der Browser.

**Verlässlichkeit:** Der Empfang ist idempotent – mehrfach übertragene identische Daten ändern nichts und erzeugen keine Protokoll-Einträge. Bricht die Datenverbindung ab, wird sie automatisch (alle paar Sekunden) neu aufgebaut, ohne dass die Kopplung verloren geht. Nach dem Update auf die Handshake-Version müssen einmal gekoppelte Geräte **einmalig neu gekoppelt** werden (die alte Kopplung wird automatisch entfernt). Bei einem **Versionsunterschied** zwischen den Geräten erscheint eine klare Warnung im Sync-Protokoll.

**Wichtig:** Damit Änderungen korrekt abgeglichen werden, sollte auf **beiden Geräten die gleiche App-Version** (mindestens 1.8.1) laufen.

## Lizenzen

Die App selbst ist eigenständiger Code. Eingebundene Fremdbibliotheken stehen unter permissiven Lizenzen (kommerzielle und private Nutzung erlaubt):

| Bibliothek | Version | Lizenz | Bezug |
|---|---|---|---|
| React | 18.3.1 | MIT | lokal (`react-18.3.1.production.min.js`) |
| ReactDOM | 18.3.1 | MIT | lokal (`react-dom-18.3.1.production.min.js`) |
| Tailwind CSS | 3.4.16 | MIT | lokal (`tailwind-3.4.16.js`, Lizenztext: `tailwind-3.4.16.LICENSE`) |
| Mammoth.js | 1.6.0 | BSD-2-Clause | lokal (`mammoth-1.6.0.browser.min.js`) |
| PDF.js | 2.16.105 | Apache-2.0 | lokal (`pdf-2.16.105.min.js` + `pdf.worker-2.16.105.min.js`) |
| PeerJS | 1.5.4 | MIT | lokal (`peerjs-1.5.4.min.js`) |
| qrcodejs | 1.0.0 | MIT | lokal (`qrcode-1.0.0.min.js`) |

Alle Bibliotheksdateien liegen versioniert im Repository und werden per SRI-Integritäts-Hash abgesichert (siehe Abschnitt Sicherheit) – dadurch funktioniert die App auch ohne Internetverbindung und ohne Abhängigkeit von CDN-Anbietern.

## Datenschutz

- **Keine zentrale Datenspeicherung:** Personen, Termine, Abwesenheiten und Einstellungen bleiben ausschließlich im lokalen Browser-Speicher (localStorage) des jeweiligen Geräts. Der Webserver (z. B. GitHub Pages) liefert nur statische Dateien aus – es gibt kein Backend, keine Datenbank und keine Konten.
- **Keine Analyse oder Weitergabe:** Es gibt kein Tracking, keine Analytics, keine Cookies und keine Telemetrie. Der einzige Abruf von Server-Daten ist die Versionsprüfung (`version.json`).
- **Geräte-Sync Ende-zu-Ende:** Die Synchronisierung läuft direkt zwischen den gekoppelten Geräten (WebRTC Data Channels, DTLS-verschlüsselt). Es fließen dabei nur verschlüsselte Datenströme – Inhalte sind für die Vermittlungsdienste (PeerJS-Signalisierung, STUN/TURN-Relays) nicht lesbar; übertragen wird die App-Datenbank, keine weiteren personenbezogenen Daten.
- **Hinweis für Teams:** Wer die App für eine Gruppe betreibt, sollte die Beteiligten informieren, dass die Einsatzdaten (Namen, Abwesenheiten) auf den beteiligten Geräten gespeichert und über die Geräte-Kopplung ausgetauscht werden.
