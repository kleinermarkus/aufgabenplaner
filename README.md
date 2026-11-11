# Aufgabenplaner (Monatsplaner & Aufgabenverteiler – Sachsen)

Eigenständige Single-File-Web-App (`index.html`) zur gerechten Aufgabenverteilung mit Feiertagsunterstützung (Sachsen).

## Nutzung

Einfach `index.html` in einem Browser öffnen (Internetverbindung nötig, da React/Tailwind per CDN geladen werden). Alle Daten bleiben lokal im Browser (localStorage).

## Geräte-Synchronisation (WebRTC Data Channels)

Über den Button **🔄 Geräte-Sync** können zwei Geräte direkt peer-to-peer synchronisiert werden:

1. **Gerät 1:** „Sync starten“ erzeugt einen 8-stelligen Sync-Code – als QR-Code **und** als Text mit Kopieren-Button.
2. **Gerät 2:** „Mit Code verbinden“ – Code eingeben (oder einfügen) und verbinden.
3. Verbunden: Orte, Zeitslots, Muster, Personen, Abwesenheiten und Termine werden automatisch in beide Richtungen übertragen.

Der Handshake läuft über das öffentliche PeerJS-Signaling; die Plandaten selbst gehen direkt Gerät-zu-Gerät (peer-to-peer) und werden auf keinem Server gespeichert.
