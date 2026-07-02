# Tarif 590 – JSON-Referenzdaten

Dieses Repository enthält die Tarif-590-Referenzdaten für komplementärmedizinische Leistungen im VVG-Bereich als JSON-Dateien.

Die Daten wurden aus der aktuellsten Excel-Datei `Tarif 590 v6.xlsx` erzeugt und stehen in zwei Varianten zur Verfügung:

- als **flache Liste** für Imports, Suche, Validierung und direkte Lookups
- als **verschachtelte Struktur nach Kapitel und Unterkapitel** für UI-Auswahlbäume und Navigation

## Dateien

| Datei | Beschreibung |
|---|---|
| `tarif_590.json` | Flache JSON-Datei mit allen Tarifpositionen als Liste. Empfohlen für Import, Suche, Validierung und Lookups nach Tarifziffer. |
| `tarif_590_nested.json` | Verschachtelte JSON-Datei, gruppiert nach Kapitel und Unterkapitel. Empfohlen für Benutzeroberflächen, Auswahlbäume und strukturierte Darstellung. |
| `Tarif 590 v6.xlsx` | Ursprüngliche Excel-Datei der Tarifziffern, sofern im Repository mitgeführt. |

## Inhalt

Die JSON-Dateien enthalten die Tarifpositionen des **Tarif 590, Version V6**.

Aktueller Umfang:

- Tarif: `590`
- Version: `V6`
- Anzahl Positionen: `217`
- Anzahl Kapitel: `10`
- Anzahl Unterkapitel: `36`
- Quelle: Excel-Liste `Tarif 590 v6.xlsx`

Die enthaltenen Leistungsdaten sind mehrsprachig aufgebaut, soweit in der Excel-Datei vorhanden:

- Deutsch
- Französisch
- Italienisch

Typische Felder pro Leistungseintrag sind unter anderem:

- Tarifnummer
- Tarifname
- Kapitel
- Unterkapitel
- Positionsnummer / Tarifziffer
- Positionstext
- Beschreibung
- Gültig-von-Datum
- Gültig-bis-Datum

## JSON-Varianten

### 1. Flache Struktur

Datei: `tarif_590_v6.json`

Diese Variante ist für technische Verarbeitung am praktischsten. Alle Positionen befinden sich in einer gemeinsamen Liste.

Typische Anwendungsfälle:

- Import in eine Datenbank
- Suche nach Tarifziffer oder Text
- Validierung beim Erfassen von Rechnungspositionen
- Migrationen und Abgleich zwischen Versionen
- Aufbau eigener Indizes, z. B. nach `positionsnummer`

Vereinfachtes Beispiel:

```json
{
  "tarif": "590",
  "version": "V6",
  "positions": [
    {
      "positionsnummer": "1200",
      "kapitel": {
        "ziffer": "1",
        "text": {
          "de": "Allgemeine Tarifziffern"
        }
      },
      "unterkapitel": null,
      "positionsText": {
        "de": "Anamnese / Untersuchung / Diagnostik / Befunderhebung, pro 5 Min."
      },
      "gueltigVon": "2016-01-01",
      "gueltigBis": "9998-12-31"
    }
  ]
}
```

### 2. Verschachtelte Struktur

Datei: `tarif_590_v6_nested.json`

Diese Variante gruppiert die Positionen nach Kapitel und Unterkapitel. Sie eignet sich besonders für die Anzeige in einer Softwareoberfläche.

Typische Anwendungsfälle:

- Auswahlbaum nach Kapitel und Unterkapitel
- strukturierte Darstellung der Tarifliste
- Navigation durch die Tariflogik
- Gruppierung in Dropdowns, Accordions oder Tree Views

Vereinfachtes Beispiel:

```json
{
  "tarif": "590",
  "version": "V6",
  "structure": "nestedByKapitelAndUnterkapitel",
  "kapitel": [
    {
      "ziffer": "1",
      "text": {
        "de": "Allgemeine Tarifziffern"
      },
      "positionen": [
        {
          "positionsnummer": "1200",
          "positionsText": {
            "de": "Anamnese / Untersuchung / Diagnostik / Befunderhebung, pro 5 Min."
          }
        }
      ],
      "unterkapitel": []
    }
  ]
}
```

## Empfehlung zur Verwendung

Für die meisten Softwarelösungen ist es sinnvoll, beide Dateien zu versionieren:

- `tarif_590_v6.json` als technische Referenz und stabile Importquelle
- `tarif_590_v6_nested.json` als abgeleitete Darstellungsstruktur für UI-Zwecke

Die flache Datei sollte als führende technische Quelle betrachtet werden. Die verschachtelte Datei kann jederzeit daraus neu erzeugt werden.

## Wichtige fachliche Hinweise

Gemäss den erhaltenen Informationen für Softwareanbieter gelten insbesondere folgende Vorgaben:

1. **Aktualisierung per 1. Januar 2026**  
   Die Änderungen müssen so umgesetzt werden, dass Kundinnen und Kunden per **01.01.2026** die aktuellen Tarifziffern zur Verfügung haben.

2. **Nicht mehr gültige Tarifziffern schliessen**  
   Nicht mehr gültige Tarifziffern dürfen ab ihrem Schliessungsdatum nicht mehr als gültige neue Leistungen verwendet werden.

3. **Tarifziffer nur einmal pro Behandlungsdatum**  
   Jede Tarifziffer darf pro Behandlungsdatum nur einmal verrechnet werden.  
   Beispiel: `1x Tarifziffer 1200` und `1x Tarifziffer 1004` sind möglich, dieselbe Tarifziffer darf jedoch nicht mehrfach am gleichen Behandlungsdatum verwendet werden.

4. **Positions-Text ist verbindlich**  
   Der Positions-Text darf grundsätzlich nicht angepasst werden. Die Bezeichnungen sind verbindlich und dürfen nicht abgeändert werden.

5. **Ausnahmen bei Tarifziffern 1310 und 1302**  
   Für die Tarifziffern `1310` und `1302` besteht eine Ausnahme bezüglich Anpassung des Positions-Textes.

## Empfehlung für Softwareintegration

Die JSON-Daten sollten nicht nur als reine Auswahlliste verwendet werden, sondern mit zusätzlichen Validierungen kombiniert werden:

- Prüfung der Gültigkeit anhand des Behandlungsdatums
- Verhinderung mehrfacher Verwendung derselben Tarifziffer am gleichen Behandlungsdatum
- Schreibschutz oder klare Warnung bei manueller Änderung verbindlicher Positions-Texte
- Sonderbehandlung der Tarifziffern `1310` und `1302`
- Versionierung der importierten Tarifdaten
- saubere Trennung zwischen Tarif-Referenzdaten und abrechnungsspezifischen Rechnungsdaten

## Beispiel: Lookup nach Tarifziffer

Bei der flachen Struktur kann ein Index nach `positionsnummer` aufgebaut werden:

```pseudo
index = positions.toDictionary(p => p.positionsnummer)
position = index["1200"]
```

Bei der verschachtelten Struktur muss entweder rekursiv gesucht oder beim Laden zusätzlich ein Index aufgebaut werden:

```pseudo
index = {}

for kapitel in tarif.kapitel:
  for position in kapitel.positionen:
    index[position.positionsnummer] = position

  for unterkapitel in kapitel.unterkapitel:
    for position in unterkapitel.positionen:
      index[position.positionsnummer] = position
```

## Beispiel: Validierung der Gültigkeit

Eine Tarifposition sollte nur auswählbar sein, wenn das Behandlungsdatum innerhalb des gültigen Zeitraums liegt:

```pseudo
valid = gueltigVon <= behandlungsdatum <= gueltigBis
```

Bei geschlossenen Tarifziffern muss das `gueltigBis`-Datum entsprechend berücksichtigt werden.

## Beispiel: Einmaligkeit pro Behandlungsdatum

```pseudo
exists = invoice.positions.any(p =>
  p.behandlungsdatum == newPosition.behandlungsdatum &&
  p.tarifziffer == newPosition.tarifziffer
)

if exists:
  reject("Diese Tarifziffer wurde an diesem Behandlungsdatum bereits verwendet.")
```

## Datenherkunft und Verantwortung

Diese JSON-Dateien sind eine technische Umwandlung der bereitgestellten Excel-Tarifliste. Massgebend bleiben die offiziellen Tarifunterlagen und die von den zuständigen Stellen kommunizierten Vorgaben.

Bei fachlichen Änderungen sollte die Excel-Quelle erneut importiert und die JSON-Dateien neu generiert werden.

## Änderungsvermerk 2026

Die zugestellten Unterlagen enthalten Änderungen für das Jahr 2026. In der Excel-Datei sind die Änderungen gemäss Begleitinformation rot markiert. Zusätzlich wurde eine kompakte Änderungsliste angekündigt.

Für eine vollständige fachliche Nachführung sollten sowohl die Excel-Tabelle als auch die kompakte Änderungsliste geprüft werden.
