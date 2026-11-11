# Aufgabenplaner (Monatsplaner & Aufgabenverteiler – Sachsen)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung (Sachsen).

## Nutzung

Einfach `index.html` in einem Browser öffnen (Internetverbindung nötig, da React/Tailwind per CDN geladen werden). Alle Daten bleiben lokal im Browser (localStorage).

## Schichtarbeit (3-Schicht-System)

Zeitslots werden automatisch einer Schicht zugeordnet: **Frühschicht** (06–14 Uhr), **Spätschicht** (14–22 Uhr) und **Nachtschicht** (22–06 Uhr). Die Schicht wird als Badge in der Slot-Verwaltung und der Qualifikations-Matrix angezeigt.

In **Personen & Matrix** kann pro Person hinterlegt werden, ob sie im 3-Schicht-System arbeitet und bei welchen Schichten sie für Aufgaben zur Verfügung steht. Die automatische Zuteilung, die Empfehlungen und die manuelle Auswahl berücksichtigen diese Einschränkung (nicht verfügbare Personen werden mit der Begründung „Schicht: …“ gekennzeichnet).

## Geräte-Synchronisation (WebRTC Data Channels)

Über den Button **🔄 Geräte-Sync** können zwei Geräte direkt peer-to-peer synchronisiert werden:

1. **Gerät 1:** „Sync starten“ erzeugt einen 8-stelligen Sync-Code – als QR-Code **und** als Text mit Kopieren-Button.
2. **Gerät 2:** „Mit Code verbinden“ – Code eingeben (oder einfügen) und verbinden.
3. Verbunden: Orte, Zeitslots, Muster, Personen, Abwesenheiten und Termine werden automatisch in beide Richtungen übertragen.

Der Handshake läuft über das öffentliche PeerJS-Signaling; die Plandaten selbst gehen direkt Gerät-zu-Gerät (peer-to-peer) und werden auf keinem Server gespeichert.
