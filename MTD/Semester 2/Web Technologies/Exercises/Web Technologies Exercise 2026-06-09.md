#web_technologies 

# Übung 10 – Medienverarbeitung

WET2UE Web Technologies | 09.06.2026 | Wolfgang Hochleitner | Abgabe

In dieser Übung soll eine Applikation, basierend auf `fhooe/router-skeleton`, erstellt werden, die eine Liste an
Produkten aus einer Datenbank in die Formate XML, JSON und PDF exportieren kann. Alle drei exportierten Formate werden
im Verzeichnis `/public` gespeichert.

## Den Startercode zum Laufen bringen

Zunächst muss der Startercode im Verzeichnis `exporter` lauffähig gemacht werden. Es handelt sich dabei um ein auf *fhooe/router-skeleton* basierendes Projekt. Folgende Dinge sind zu tun:

1. Den Basispfad in `public/index.php` in Zeile 50 anpassen.
2. Dependencies installieren: zunächst eine Bash-Shell in den Docker-Container öffnen: `docker exec -it webapp /bin/bash`, dann in den Ordner des Projekts und in den Unterordner `exporter` wechseln: `cd ue10-YourTeamName/exporter`. Nun mit `composer install` die Dependencies installieren. Dies erzeugt den `vendor`-Ordner.
3. Projekt wie gewohnt aufrufen (zum Ordner `public` navigieren). Z.B. http://localhost:8080/ue10-YourTeamName/exporter/public/. Die Index-Route wird angezeigt.
4. Sollte eine Fehlermeldung über fehlende Rechte zum Schreiben der Log-Datei angezeigt werden, erteilt dem gesamten Ordner `ue10-YourTeamName` und allen Unterordnern mit folgendem Befehl entsprechende Rechte:
   ```bash
   chmod -R 777 .
   ```

## Datenbank erzeugen

Um eure Datenbank mit Beispieldaten zu befüllen, ruft die Route `/createdb` auf. Sie erzeugt zunächst die Datenbank "ue10_products", legt dann eine Tabelle "product" and und fügt in diese drei Beispielprodukte ein. Die Tabelle mit den Beispieldaten sieht wie folgt aus:

| id (PK) | item_nr  | product_name    | product_description                           | available_quantity | price  |
|---------|----------|-----------------|-----------------------------------------------|--------------------|--------|
| 1       | 11234115 | Game Controller | Wireless Game Controller für PC               | 234                | 39.99  |
| 2       | 54325324 | Rennlenkrad     | Rennlenkrad für PC/Xbox/PS mit Force Feedback | 127                | 329.00 |
| 3       | 21335689 | 4K Monitor      | 28" 4K Gaming Monitor UHD IPS                 | 1234               | 513.28 |

## Den Exporter umsetzen

Nachdem das Basisprojekt nun läuft, kann mit der Umsetzung des Exporters begonnen werden. Anpassungen sind zunächst in `public/index.php` für die Routen erforderlich. Der eigentliche Export in unterschiedliche Formate erfolgt in der Klasse `src/WET2UE10/ProductExporter.php`.

### Routen anpassen

Die Routen `/xml`, `/json` und `/pdf` sind bereits in `public/index.php` vorhanden. Legt im Callback der jeweiligen Route ein Objekt der Klasse `ProductExporter` an und übergebt ihm das Latte-Objekt, damit ihr später den Output anzeigen könnt.

Ruft in weiterer Folge die Methode `export(ExportFormat $format, string $filename)` auf.

Der erste Parameter ist ein [Enum](https://www.php.net/manual/de/language.types.enumerations.php), der genau 3 Werte, nämlich XML, JSON oder PDF, annehmen kann. Damit lässt sich der gewünschte Typ zum Export steuern. Der Enum hat dabei (z. B. gegenüber einem String) den Vorteil, dass außer diesen drei Werten nichts zulässig ist. Somit muss hier keine Überprüfung auf mögliche ungültige Werte erfolgen.

Der zweite Parameter ist der Dateiname. Wählt einen beliebigen Namen mit der korrekten Endung für das jeweilige Format.

Ruft schließlich noch `displayOutput()` auf, um nach dem Export ein Template anzeigen zu können.

### Klasse `ProductExporter`

Diese Klasse übernimmt die Konvertierung in verschiedene Formate. Im Konstruktor wird das Latte-Objekt bereits einer Eigenschaft zugewiesen und die Datenbank initialisiert.

#### Methode `export(ExportFormat $format, string $filename)`

Diese Methode wird in jeder der Routen aufgerufen und startet den Export. Der Parameter steuert das gewünschte Format.

Ruft anhand des übergebenen Arguments für `$format` die jeweilige private Exportmethode auf:

- `exportXML(array $products, string $filename)` um ein XML-Dokument zu erzeugen,
- `exportJSON(array $products, string $filename)` um ein JSON-Dokument zu erzeugen,
- `exportPDF(array $products, string $filename)` um ein PDF-Dokument zu erzeugen,

Diese Methoden erhalten ein Array mit den Produkten (`$products`). Um dieses zu bekommen, müsst ihr die Produkte aus der Datenbank zuvor abfragen.

#### Methode `exportXML(array $products, string $filename)`

Diese Methode erzeugt eine XML-Datei. Verwendet dazu, wie in der Vorlesung gezeigt, `XMLWriter` oder `DOM` um eine XML-Struktur zu erstellen und in eine Datei zu speichern.

Die fertige XML-Datei soll dabei wie folgt aufgebaut sein:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
    <product>
        <item_nr>11234115</item_nr>
        <product_name>Game Controller</product_name>
        <product_description>Wireless Game Controller für PC</product_description>
        <available_quantity>234</available_quantity>
        <price>39.99</price>
    </product>
    <product>
        <item_nr>54325324</item_nr>
        <product_name>Rennlenkrad</product_name>
        <product_description>Rennlenkrad für PC/Xbox/PS mit Force Feedback</product_description>
        <available_quantity>127</available_quantity>
        <price>329.00</price>
    </product>
    <product>
        <item_nr>21335689</item_nr>
        <product_name>4K Monitor</product_name>
        <product_description>28&quot; 4K Gaming Monitor UHD IPS</product_description>
        <available_quantity>1234</available_quantity>
        <price>513.28</price>
    </product>
</products>
```

#### Methode `exportJSON(array $products, string $filename)`

Diese Methode erzeugt eine JSON-Datei. Verwendet `json_encode()`, um eine JSON-Struktur zu erzeugen, und schreibt diese mit `file_put_contents()` in eine Datei.

Die JSON-Datei soll folgendermaßen aufgebaut sein:

```json
{
  "products": [
    {
      "item_nr": "11234115",
      "product_name": "Game Controller",
      "product_description": "Wireless Game Controller für PC",
      "available_quantity": "234",
      "price": "39.99"
    },
    {
      "item_nr": "54325324",
      "product_name": "Rennlenkrad",
      "product_description": "Rennlenkrad für PC/Xbox/PS mit Force Feedback",
      "available_quantity": "127",
      "price": "329.00"
    },
    {
      "item_nr": "21335689",
      "product_name": "4K Monitor",
      "product_description": "28\" 4K Gaming Monitor UHD IPS",
      "available_quantity": "1234",
      "price": "513.28"
    }
  ]
}
```

#### Methode `exportPDF(array $products, string $filename)`

Diese Methode erzeugt eine PDF-Datei. Verwendet Dompdf in Kombination mit einem Latte-Template, um eine Überschrift und eine Tabelle mit den Produkten zu erzeugen.

Die PDF-Datei soll als Tabelle so aufgebaut sein:

| <img src="pdf.png" alt="Das PDF enthält die Produktdaten in Form einer Tabelle." width="800"> |
|:---------------------------------------------------------------------------------------------:|
|                  **Das PDF enthält die Produktdaten in Form einer Tabelle.**                  |

Die Tabelle muss optisch nicht genauso aussehen, es soll sich jedoch um eine Tabelle mit diesen fünf Spalten handeln.

## Tipps und Richtlinien

- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.