# FM-FT8 - FT8 über FM

**Mein Vorschlag für ein Experiment: normales FT8 über gewöhnliche FM-Funkgeräte auf 2 m.**

[English](README.md) · Artikel: <https://dl1tj.de/de/fmft8.html> · Skimmer: <https://fmft8.db0rbx.de>

Das ist ein persönlicher Vorschlag von mir, Tobias Johnson, DL1TJ. Es ist kein Standard,
kein offizielles Projekt und mit niemandem abgestimmt. Ich schreibe ihn offen auf, damit
andere ihn ausprobieren, prüfen und mir sagen können, wo er falsch liegt.

> **Stand: eine Idee, noch nicht auf dem Band erprobt.** Ich habe so noch keinen Kontakt
> gemacht und nichts gemessen. Jede Angabe zu Reichweite oder Leistung ist das, was ich
> erwarte, nicht das, was ich weiß.

Die weiteren Dokumente in diesem Repository sind vorerst auf Englisch; die
Schritt-für-Schritt-Anleitung gibt es auf Deutsch auf meiner
[Webseite](https://dl1tj.de/de/fmft8.html).

## Die Idee

FT8 ist die zugänglichste digitale Betriebsart, die wir haben: feste Nachrichten,
automatischer Ablauf, Kontakte, die sich selbst abschließen und loggen. Auf 2 m läuft FT8
in SSB, und ein SSB-fähiger 2-m-Transceiver kostet ein Vielfaches eines FM-Handfunkgeräts.
Gerade die, denen FT8 als erster Schritt am meisten Freude machen würde - Einsteiger, in
Deutschland besonders Inhaber der Klassen N und E - können es deshalb oft nicht ausprobieren.

Aber FT8 ist Audio, und ein FM-Funkgerät kann Audio übertragen. Mein Vorschlag:
**normales, unverändertes FT8** mit **WSJT-X** über ein **gewöhnliches FM-Handfunkgerät**,
und eine vereinbarte Anruffrequenz, um einander zu finden.

```
 FM-Funkgerät  <-->  Soundkarte + PTT  <-->  Computer  <-->  WSJT-X
 (2 m,               (Audiokabel,            (USB,           (dekodiert, sendet,
  Narrow FM)          Hardware-PTT)           genaue Uhr)     loggt)
```

## Was zu erwarten ist

Das ist ein Einstieg in den digitalen Betrieb, kein Weak-Signal-Verfahren.

- **Kein Weak-Signal-Vorteil.** FT8 in SSB dekodiert weit unter dem Rauschen. Über einen
  FM-Empfänger geht das größtenteils verloren. Rechne etwa mit der Reichweite von
  FM-Sprechfunk.
- **Immer nur eine Station.** FM-Empfänger rasten auf das stärkste Signal ein; zwei
  Stationen im selben Zeitschlitz blockieren einander.
- **Alles andere ist echtes FT8.** Dieselbe Software, dieselben Nachrichten, dasselbe
  Timing und Logging wie in SSB und auf Kurzwelle - was du lernst, gilt dort unverändert.

Warum das so ist: [docs/TECHNICAL_BACKGROUND.md](docs/TECHNICAL_BACKGROUND.md).

## Vorgeschlagener Aufbau

| Parameter | Vorschlag |
|---|---|
| Frequenz | 144,575 MHz, Simplex |
| Modulation | Narrow FM, Kanalraster 12,5 kHz |
| Digitales Protokoll | Normales FT8, unverändert |
| Software | WSJT-X |
| Zeitsteuerung | Normales FT8: 15-Sekunden-Slots, Computeruhr synchronisiert |
| PTT | Per Hardware gesteuert; VOX nur als Notlösung |
| Audio | Kabelgebundenes Interface |
| Sendeleistung | Die kleinste, mit der du ankommst |

144,575 MHz ist mein Vorschlag für eine experimentelle Anruffrequenz. Sie ist weder FT8
zugewiesen noch koordiniert und nicht exklusiv. Prüfe vor dem Senden die Aktivität vor Ort,
deine Zulassung und den aktuellen Bandplan der IARU-Region 1, und höre zuerst.
Siehe [docs/FREQUENCY_PLAN.md](docs/FREQUENCY_PLAN.md).

## Wo es steht

- Der Vorschlag und eine Schritt-für-Schritt-Anleitung stehen auf meiner Webseite, auf
  [Deutsch](https://dl1tj.de/de/fmft8.html) und [Englisch](https://dl1tj.de/fmft8.html).
- Ein Skimmer (nur Empfang) an meiner Station DB0RBX in Regensburg (JN69ba) hört rund um
  die Uhr auf 144,575 MHz FM: <https://fmft8.db0rbx.de>
- Noch offen: das Referenz-Interface, ein Kontakt auf dem Band, jede Messung.

## Was ich vorhabe

Drei Stufen, die ich getrennt halte:

- **A. Normales FT8 über FM (jetzt).** Ein Referenz-Interface bauen, die ersten Kontakte
  machen und messen, wie gut es wirklich funktioniert. Der
  [Testplan](testing/TEST_PLAN.md) nennt, was ich herausfinden will.
- **B. Eine digitale Betriebsart für FM (eine Idee für später).** FT8 wurde für SSB
  entwickelt. Ein Protokoll für den FM-Kanal könnte eine Nachricht in ein bis zwei Sekunden
  statt in 13 senden, mit Vorwärtsfehlerkorrektur, in einem schmalen Kanal, und trotzdem
  mit günstigen Handfunkgeräten funktionieren. Ein anderer Weg wären LoRa-Module. Nichts
  davon gibt es.
- **C. Digitaler Amateurfunk für alle (der weite Blick).** Ein Interface, das jeder
  nachbauen kann, Notizen dazu, welche Funkgeräte funktionieren, echte Empfangsergebnisse,
  Dokumentation auf Deutsch und Englisch.

## Probier es aus und sag mir Bescheid

Wenn du es ausprobierst, würde ich gern hören, wie es gelaufen ist, ob es geklappt hat oder
nicht: als Issue hier oder per E-Mail an <dl1tj@darc.de>, auf Deutsch oder Englisch.
Korrekturen an den technischen Überlegungen sind genauso willkommen.

## Dank und Lizenz

FT8 und WSJT-X sind das Werk von Joe Taylor (K1JT), Steve Franke (K9AN), Bill Somerville
(G4WJS) und der WSJT Development Group. Dieser Vorschlag ist mein eigener und steht in
keiner Verbindung zu ihnen, dem DARC oder der IARU.

Texte in diesem Repository: [CC BY 4.0](LICENSE).

Tobias Johnson, DL1TJ - <https://dl1tj.de>
