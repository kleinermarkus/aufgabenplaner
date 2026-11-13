# 📘 Tutorial: Einrichtung & wichtigste Funktionen

Schritt-für-Schritt-Anleitung für den Aufgabenplaner. Die ausführliche Funktions- und Technikkreferenz steht in der [README.md](README.md).

---

## Inhalt

1. [Beim ersten Start](#1-beim-ersten-start)
2. [Stammdaten einrichten](#2-stammdaten-einrichten)
3. [Personen & Qualifikationen](#3-personen--qualifikationen)
4. [Schichtrhythmus hinterlegen](#4-schichtrhythmus-hinterlegen)
5. [Gruppen anlegen](#5-gruppen-anlegen)
6. [Wiederkehrende Muster anlegen](#6-wiederkehrende-muster-anlegen)
7. [Der Monatsrhythmus: 6 Schritte](#7-der-monatsrhythmus-6-schritte)
8. [Manuell zuweisen & Empfehlung](#8-manuell-zuweisen--empfehlung)
9. [Termine nachträglich ändern](#9-termine-nachträglich-ändern)
10. [Zwei Geräte koppeln (Sync)](#10-zwei-geräte-koppeln-sync)
11. [Als App installieren (PWA)](#11-als-app-installieren-pwa)
12. [Statistik & Fairness](#12-statistik--fairness)
13. [Backup & Datenschutz](#13-backup--datenschutz)
14. [Tipps & häufige Fragen](#14-tipps--häufige-fragen)

---

## 1. Beim ersten Start

Die App besteht aus einer einzigen Datei (`index.html`) und speichert **alle Daten nur im Browser** – es gibt keinen Server, der Ihre Daten kennt.

**Beim allerersten Öffnen** legt die App automatisch **anonyme Beispieldaten** an (6 Beispielpersonen, 2 Gruppen, Beispielmuster, eine Beispiel-Abwesenheit, einige Termine). Damit können Sie alles gefahrlos ausprobieren.

**So kommen Sie die Beispieldaten wieder los:**

- Am einfachsten: Importieren Sie zum ersten Mal Ihre **Abwesenheiten** oder **Ereignisse**. Vor dem Import fragt ein **Pop-up**, ob die Beispieldaten gelöscht werden sollen – nach Bestätigung sind sie weg und Ihre echten Daten werden sauber importiert.
- Alternativ: Löschen Sie die Demo-Einträge einzeln, oder setzen Sie in **⚙️ Stammdaten & Export → Gefahrenzone** alle Daten zurück.

> Bereits genutzte Apps werden nie automatisch überschrieben – die Beispieldaten entstehen nur bei leerem Browser-Speicher.

---

## 2. Stammdaten einrichten

Alles Grundlegende finden Sie im Tab **⚙️ Stammdaten & Export**:

1. **Orte** anlegen (z. B. „Hauptsaal", „Nebenzimmer") – wo finden Aufgaben statt?
2. **Zeitslots** anlegen (Wochentag + Uhrzeit, z. B. „Dienstag 09:00") – wann finden Aufgaben statt?
3. **Bundesland** wählen (für die Feiertage im Kalender, Standard: Sachsen).

Orte und Zeitslots brauchen Sie, bevor Sie Muster und Termine anlegen können – die Qualifikations-Matrix bezieht sich immer auf eine Kombination aus Zeitslot und Ort.

---

## 3. Personen & Qualifikationen

Im Tab **👥 Personen**:

1. **Person hinzufügen**: Name eingeben – jede Person erhält automatisch eine **eindeutige Farbe**, die im ganzen Kalender verwendet wird.
2. **Qualifikations-Matrix ausfüllen**: Klicken Sie pro Person die Zellen an (Zeitslot × Ort), die die Person übernehmen kann. Nur qualifizierte Personen werden vorgeschlagen und automatisch eingeteilt.
3. **Abwesenheiten** (Urlaub, Krankheit …) können Sie hier pro Person manuell eintragen – oder praktischer per Import (siehe Schritt 1 im Monatsrhythmus).

---

## 4. Schichtrhythmus hinterlegen

Wer extern im 3-Schicht-System arbeitet, wird automatisch gesperrt, solange eine Schicht läuft:

1. Tab **👥 Personen** → bei der Person **Schichtrhythmus aktivieren**.
2. **Startdatum** angeben: der Tag, an dem der Rhythmus beginnt.
3. **Reihenfolge anklicken** (Kacheln): jede Kachel schaltet weiter zwischen Frei → Früh → Spät → Nacht. Beispiel für einen 8-Tage-Rhythmus: `F F S S N N – –`.
4. Das Muster läuft ab dem Startdatum **unbegrenzt weiter** – eine Vorschau zeigt die nächsten 7 Tage.

**Blockierungen:** Früh 06–14 Uhr · Spät 14–22 Uhr · Nacht 22–06 Uhr **und am Folgetag bis 14 Uhr** (wer nachts arbeitet, kann am nächsten Morgen keine Aufgabe übernehmen). Eine angenommene Früh-Schicht blockiert den Vormittag, aber der Nachmittag bleibt frei.

In der Verfügbarkeits-Liste steht dann z. B. „Schicht: Früh (06–14 Uhr)" als Grund, warum jemand nicht wählbar ist.

---

## 5. Gruppen anlegen

Im Tab **⚙️ Stammdaten & Export** können Sie **Gruppen** erstellen (z. B. „Gruppe Nord") und Personen zuordnen.

- Einzel- und wiederkehrende Termine können **optional** für eine oder mehrere Gruppen relevant sein.
- **Gruppentermine laufen „außer Konkurrenz"**: Sie werden automatisch angelegt, bekommen aber **nie automatisch eine Person** – die Zuteilung erfolgt manuell. Sie zählen auch nicht in die Verteilungsbilanz anderer Termine.
- Wer an einem Tag einen Gruppentermin hat, wird an dem Tag nicht für allgemeine Termine eingeteilt.
- Im Kalender sind Gruppentermine **blau** (bei mehreren Gruppen **dunkelblau**) und zeigen links einen Ein-Buchstaben-Chip je Gruppe; ohne Zuteilung steht dort der Gruppenname bzw. „Gruppen".

---

## 6. Wiederkehrende Muster anlegen

Im Tab **🔄 Muster** legen Sie fest, welche Termine regelmäßig entstehen:

1. **Zeitslot + Ort** wählen.
2. **Häufigkeit**:
   - **Wöchentlich** – jede Woche am gleichen Wochentag.
   - **Alle 2 Wochen** – über die Kalenderwochen-Parität (gerade/ungerade KW).
   - **Monatlich** – entweder an einem **festen Tag des Monats** (z. B. dem 15.) oder am **x. Wochentag im Monat** (z. B. „1. Dienstag", auch „letzter" wählbar).
3. Optional: **Gruppen** zuordnen, für die der Termin relevant ist.

Beim Klick auf **„3. Termine generieren"** (siehe nächstes Kapitel) werden aus allen Mustern die Termine des angezeigten Monats erzeugt – inklusive Gruppenterminen, mit Duplikatschutz auch über Monatsgrenzen hinweg.

---

## 7. Der Monatsrhythmus: 6 Schritte

Die Schaltflächen über dem Kalender sind genau in der Reihenfolge nummeriert, in der Sie jeden Monat vorgehen:

| Schritt | Schaltfläche | Was passiert |
|---|---|---|
| 1️⃣ | **Abwesenheiten importieren** | PDF/DOCX mit Abwesenheiten einlesen – neue Zeiträume werden je Person zugeordnet |
| 2️⃣ | **Ereignisse importieren** | Zusätzliche Termine (z. B. Versammlungsereignisse) aus DOCX/TXT einlesen – erscheinen wie Feiertage im Kalender |
| 3️⃣ | **Termine generieren** | Alle wiederkehrenden Termine des Monats anlegen (inkl. Gruppentermine) |
| 4️⃣ | **Personen zuteilen** | Faire automatische Verteilung auf alle offenen Termine |
| 5️⃣ | **Zuteilungen löschen** | Alle Zuteilungen des Monats freigeben (z. B. nach späten Abwesenheits-Imports) |
| 6️⃣ | **Termine löschen** | Alle Termine des angezeigten Monats entfernen (Neuanfang) |

Nach jedem Import erscheint kurz ein **grünes Banner**, das die Änderungen auflistet.

**Typischer Monatsablauf:** 1 → 2 → 3 → 4. Bereits zugewiesene Termine lässt „4. Personen zuteilen“ unangetastet – nur offene Termine werden gefüllt. Wenn Sie **alles** neu verteilen lassen wollen (z. B. weil Abwesenheiten nachkommen): erst 5 (alle Zuteilungen des Monats freigeben, auch manuelle), dann 4 – und manuelle Sonderfälle danach wieder zuweisen (siehe Kapitel 8).

---

## 8. Manuell zuweisen & Empfehlung

1. Im Tab **📅 Planer** den **Tag antippen** – rechts öffnet sich das Panel.
2. Mit **„+ Aufgabe anlegen"** einen Termin für diesen Tag erstellen (Zeitslot/Ort wählen, optional Gruppen).
3. Unter dem Termin sehen Sie die **Verfügbarkeits-Liste**: Jede Person ist ein klickbarer Button.
   - **Klickbar** = verfügbar → Klick setzt die Person direkt ein.
   - **Ausgegraut mit Grund** = nicht verfügbar, z. B. „Abwesend/Urlaub", „Schicht: Früh (06–14 Uhr)", „Nicht für diesen Ort/Zeitslot qualifiziert", „Gruppentermin an diesem Tag".
4. Der **⭐ Empfehlung**-Vorschlag berücksichtigt bereits alle vorhandenen Einteilungen (Vergangenheit **und** Zukunft) – ein Klick übernimmt ihn.

**Nach welchen Regeln wird empfohlen/verteilt?** In dieser Reihenfolge:

1. **Längste Pause** seit der letzten und bis zur nächsten Einteilung (in beide Richtungen – wer gerade erst eingeteilt wurde, wird geschont).
2. **Geringste Gesamtzahl** an Einteilungen.
3. **Wochentags-Balance** (wer an diesem Wochentag selten dran war, wird bevorzugt).
4. **Alphabetisch** als letzter Gleichstand-Tiebreak.

Gruppentermine zählen dabei nicht mit.

---

## 9. Termine nachträglich ändern

- **Datum ändern**: Jeder Termin hat im Panel ein **Datumsfeld** – einfach ändern, der Kalender springt mit. Fällt der Termin damit auf einen anderen Wochentag, fragt ein Banner, ob ein **vorhandener Zeitslot** des neuen Wochentags genutzt oder ein **neuer Zeitslot** angelegt werden soll – so bleiben die Qualifikationsprüfungen korrekt.
- **Gruppen ändern**: Die Gruppen-Chips am Termin lassen sich jederzeit anklicken und umschalten (auch mehrere).
- **Zeitslot/Ort ändern**: ebenfalls im Panel möglich.
- **Zuteilung ändern**: andere Person anklicken oder mit ✕ freigeben – unbesetzte allgemeine Termine erscheinen **rot** im Kalender.

---

## 10. Zwei Geräte koppeln (Sync)

Daten lassen sich direkt zwischen zwei Geräten abgleichen (WebRTC, ohne Server):

1. Auf dem **ersten Gerät**: Kopfzeile → **🔄 Sync** → **„Sync starten (Code erzeugen)"**.
2. **Schlüssel übertragen** – per **QR-Code scannen** oder per **„📋 Schlüssel kopieren"** und auf dem anderen Gerät einfügen (Textfeld). Der Schlüssel hat die Form `Code.Schlüssel` und berechtigt zum Datenabgleich – nur mit dem eigenen zweiten Gerät teilen.
3. Auf dem **zweiten Gerät**: **„Mit Code verbinden"** → Schlüssel einfügen/scannen.
4. Fertig: Ab jetzt werden Änderungen **automatisch in beide Richtungen** synchronisiert – auch **Löschungen**.

**Wichtig zu wissen:**

- Die Kopplung bleibt **dauerhaft gespeichert** – beim nächsten Öffnen verbindet sich das Gerät automatisch wieder und synchronisiert im Hintergrund.
- Über **„📜 Sync-Protokoll"** im Sync-Dialog können Sie nachvollziehen, was übertragen wurde.
- Koppeln Sie eine Demo-App an eine bereits genutzte App, werden die Beispieldaten beim ersten Abgleich automatisch entfernt.
- Trennen lässt sich die Kopplung jederzeit über **„🔑 Verbindung trennen (Kopplung löschen)"**.

---

## 11. Als App installieren (PWA)

Die App ist als **PWA installierbar** und funktioniert danach auch **offline**:

- **Android/Chrome**: Menü (⋮) → „Zum Startbildschirm hinzufügen" / „App installieren".
- **iPhone/Safari**: Teilen-Symbol → „Zum Home-Bildschirm".
- **Windows/Mac (Chrome/Edge)**: Install-Symbol in der Adressleiste.

**Updates:** Erscheint eine neue Version auf dem Server, meldet das gelbe **Update-Banner** oben „Eine neue Version ist verfügbar – bitte aktualisieren." Ein Klick auf **„Jetzt aktualisieren"** leert den Cache und lädt die neue Version. Die aktuelle Versionsnummer steht klein neben dem Sync-Button in der Kopfzeile.

> Der Erste-Start braucht einmalig Internet (Laden der Bibliotheken); danach läuft alles offline. Hinweis für das zweite Gerät: Nach einem App-Update beide Geräte aktualisieren und – falls der Sync zickt – die Kopplung einmal neu anlegen.

---

## 12. Statistik & Fairness

Im Tab **📊 Statistik** sehen Sie pro Person, wie oft sie eingeteilt wurde – aufgeschlüsselt nach Ort/Zeitslot, inklusive Matrix-Übersicht der Qualifikationen. So lassen Sie prüfen, ob die Verteilung ausgeglichen ist.

Die automatische Zuteilung (Schritt 4) verteilt nach den Fairness-Regeln aus Kapitel 8. Wenn jemand „zu oft" dran scheint: Meist steckt eine Sperre dahinter (Abwesenheit, Schicht, fehlende Qualifikation) – die Verfügbarkeits-Liste im Panel zeigt die Gründe für jeden Termin.

---

## 13. Backup & Datenschutz

- **Backup Exportieren (JSON)** (⚙️ Stammdaten & Export): erzeugt eine Datei mit allen Daten – regelmäßig sichern!
- **Backup Importieren (JSON)**: stellt ein Backup wieder her.
- **Datenschutz:** Alle Daten bleiben im Browser Ihres Geräts. Die einzige Verbindung zu anderen Rechnern ist die von Ihnen selbst gestartete Gerätekopplung (verschlüsselt, nur mit Ihrem Schlüssel). Es gibt keine Server, keine Analyse, keine Cookies.

---

## 14. Tipps & häufige Fragen

**„Warum wird Person X nicht eingeteilt?"**
Im Panel den Tag antippen → Verfügbarkeits-Liste lesen. Häufigste Gründe: fehlende Qualifikation in der Matrix, Abwesenheit, aktive Schicht oder Gruppentermin an dem Tag.

**„Die automatische Zuteilung soll bereits manuell geplante Termine berücksichtigen."**
Macht sie – sowohl manuelle als auch automatische Einteilungen fließen ein, egal ob in der Vergangenheit oder Zukunft. Nur Gruppentermine bleiben außen vor.

**„Termine wurden doppelt generiert."**
Der Duplikatschutz greift beim Generieren; bei Gruppenterminen, die schon existieren, bietet die App eine Übernahme an. Im Zweifel: Schritt 6 (Termine löschen) für den Monat, dann neu generieren.

**„Das zweite Gerät synchronisiert nicht."**
Beide Geräte auf derselben App-Version? (Versionsnummer in der Kopfzeile vergleichen, ggf. Update-Banner nutzen.) Danach einmal die Kopplung trennen und neu anlegen. Das Sync-Protokoll zeigt, was beim letzten Versuch passiert ist.

**„Ich habe etwas gelöscht, es taucht auf dem anderen Gerät wieder auf."**
Seit der Löschweitergabe werden Löschungen synchronisiert. Beide Geräte sollten dieselbe Version haben – alte Kopplungen einmal neu anlegen.

---

*Diese Anleitung bezieht sich auf die aktuelle Version der App. Details zu Technik, Lizenzen und allen Funktionen: [README.md](README.md).*
