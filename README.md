# Handoff: Improfy Musteranträge (AVGS 4Steps + § 16k AllCare)

## Auftrag an Claude Code

Dieses Verzeichnis auf GitHub pushen:

```
https://github.com/mpayinda-cyber/antraege-improfy.git
```

Das Repository ist **leer** (keine Commits). Ablauf:

```bash
cd <dieses Verzeichnis>
git init
git add .
git commit -m "Improfy Musteranträge: AVGS 4Steps und § 16k AllCare, 4 Standorte"
git branch -M main
git remote add origin https://github.com/mpayinda-cyber/antraege-improfy.git
git push -u origin main
```

Nichts umbauen, nichts konvertieren, kein Build-Setup. Die Dateien laufen so wie
sie sind direkt im Browser und sollen unverändert im Repository landen.

## Was das ist

Acht ausfüllbare Musteranträge für Improfy (Bildungsträger), als HTML-Dokumente
im A4-Format. Zwei Maßnahmen an je vier Standorten. Jeder Antrag lässt sich am
Bildschirm ausfüllen und erzeugt per Button ein PDF mit echten
AcroForm-Formularfeldern, das Empfänger in Acrobat oder im Browser weiter
ausfüllen und speichern können.

Die Dateien sind **fertige Deliverables**, keine Design-Referenzen — sie werden
so genutzt, nicht nachgebaut.

## Einstiegspunkt

`Improfy Anträge (alle).dc.html` — enthält alle acht Anträge in einer Datei,
Maßnahme und Standort werden über die Leiste oben umgeschaltet. Diese Datei ist
die, die veröffentlicht wird: sie referenziert keine Nachbardateien und
erzeugt ihr PDF vektorbasiert, ohne externe Bilddateien.

`Übersicht Anträge.dc.html` ist ein internes Inhaltsverzeichnis und verlinkt die
acht Einzeldateien — funktioniert nur, wenn das ganze Verzeichnis vorliegt.

## Dateien

| Datei | Inhalt |
| --- | --- |
| `Improfy Anträge (alle).dc.html` | Alle 8 Anträge, umschaltbar. Publish-sicher. |
| `Übersicht Anträge.dc.html` | Internes Inhaltsverzeichnis mit Links |
| `Antrag AVGS 4Steps.dc.html` | 4Steps Köln |
| `Antrag AVGS 4Steps Hamburg.dc.html` | 4Steps Hamburg |
| `Antrag AVGS 4Steps Mannheim.dc.html` | 4Steps Mannheim |
| `Antrag AVGS 4Steps Frankfurt.dc.html` | 4Steps Frankfurt |
| `Antrag 16k AllCare Koeln.dc.html` | § 16k Köln |
| `Antrag 16k AllCare Hamburg.dc.html` | § 16k Hamburg |
| `Antrag 16k AllCare Mannheim.dc.html` | § 16k Mannheim |
| `Antrag 16k AllCare Frankfurt.dc.html` | § 16k Frankfurt |
| `doc-page.js` | Seitenrahmen-Komponente (A4, Kopf/Fuß, Druckgeometrie) |
| `support.js` | Laufzeit für die `.dc.html`-Dateien |
| `assets/` | Logo und die PDF-Hintergründe der Einzeldateien |

Die Einzeldateien brauchen `assets/antrag-sheet-<stadt>.png` bzw.
`assets/antrag16k-sheet-<stadt>.png` als PDF-Hintergrund. Die Sammeldatei
braucht keine — sie zeichnet das PDF direkt aus dem Layout.

## Stammdaten

### AVGS 4Steps
| Standort | Maßnahmenummer | Adresse |
| --- | --- | --- |
| Köln | 357/19/25 | Rolshover Str. 45, 51105 Köln |
| Hamburg | 123/8306/25 | Hammerbrookstr. 90, 20097 Hamburg |
| Mannheim | 644/40/25 | M7 19-20, 68161 Mannheim |
| Frankfurt am Main | 419/405/25 | Elbestr. 48, 60329 Frankfurt am Main |

### § 16k AllCare
| Standort | Maßnahmenummer | Telefon |
| --- | --- | --- |
| Köln | 962/5014/24 | 0221.170.027.61 |
| Hamburg | 962/5014/24 | 040.6758.58.96 |
| Mannheim | 644/95/26 | 0621.178.515.62 |
| Frankfurt am Main | 962/5014/24 | 069.955.079.57 |

Die Vollmacht lautet in allen acht Anträgen auf Improfy GmbH, Rolshover Str. 45,
51105 Köln — das ist so gewollt und keine Inkonsistenz.

Noch offen: **Berlin** (4Steps 962/36/25) und **Stuttgart** (4Steps 677/1011/25)
sind noch nicht angelegt, dafür fehlen Adresse und Telefonnummer.

## Gestaltung

Corporate Design aus dem Improfy-Webauftritt, Farben direkt aus der Logodatei
gemessen:

- Ink `#000000` — sämtlicher Text, Feldlinien, Trennlinien. Bewusst reines
  Schwarz statt Grau, damit Ausdrucke besser lesbar sind.
- Akzent `#84faa1` (Mint) — 3px-Linien unter Kopfzeile und Abschnittsköpfen,
  über der Vollmacht; Aktivzustand in der Umschaltleiste.
- Flächen `#f6f6f6`, Rasterlinien `#e6e5e6`, Feldfüllung `#fbfbfb`,
  Fokus `#4bbf7a` auf `#f2fbf5`.
- Schrift Manrope (Google Fonts), 400–800. Fließtext 12,5px/1,55.
  Labels 10px, 700, `letter-spacing: .06em`, Versalien.
- Keine runden Ecken, keine Schatten im Dokument.

Seitengeometrie: A4, Rand 13mm, jeder Antrag passt auf genau eine Seite. Die
Höhen sind knapp austariert — längere Adressen oder größere Schrift lassen den
Antrag auf eine zweite Seite laufen.

## PDF-Export

Zwei Verfahren, historisch gewachsen:

**Sammeldatei (bevorzugt):** zeichnet das PDF mit pdf-lib vektorbasiert neu,
indem es Füllungen, Rahmen und Text aus dem Live-Layout ausliest (Zeichen für
Zeichen über `Range.getBoundingClientRect()`, damit Zeilenumbrüche und
Sperrsatz stimmen). Text bleibt im PDF markierbar, ~180 KB, keine
Bildabhängigkeit.

**Einzeldateien:** legen die Formularfelder über ein 300-dpi-Abbild der Seite
(`assets/…-sheet-<stadt>.png`). Scharf im Druck, Text aber nicht markierbar.
Wichtig: **Layout- oder Textänderungen erfordern ein neu aufgenommenes
Hintergrundbild**, sonst zeigt das PDF den alten Stand.

Felder in beiden Fällen: 12 Textfelder (`antrag.*`), bei 4Steps zusätzlich 4
Checkboxen (`modul.*`). Die Unterschriftszeile ist absichtlich kein Feld.
Feldpositionen werden zur Klickzeit aus dem DOM gemessen, nie fest hinterlegt.

Eingaben werden pro Antrag getrennt in `localStorage` gehalten
(`improfy-antrag-<maßnahme>-<stadt>`).

## Fallstricke

- `doc-page` vermisst Kopf- und Fußzeile, wenn es verbunden wird — im
  DC-Kontext ist das *vor* dem Existieren der Inhalte. Ein `ResizeObserver` auf
  beide Slots löst die Neumessung aus; ohne das überlappt die Kopfzeile den
  Inhalt und der PDF-Export liest falsche Geometrie.
- Beim Veröffentlichen wird nur die geöffnete Datei publiziert. Deshalb die
  Sammeldatei — die Übersicht führt veröffentlicht ins Leere.
