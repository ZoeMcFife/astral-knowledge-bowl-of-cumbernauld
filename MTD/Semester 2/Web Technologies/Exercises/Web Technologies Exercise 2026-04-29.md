#web_technologies #php 

# Übung 4 – Objektorientiertes PHP

WET2UE Web Technologies | 29.04.2026 | Wolfgang Hochleitner | Code-along

Diese Übung markiert den Start in die objektorientierte Entwicklung mit PHP. In einer Code-along-Session sollen die wichtigsten Aspekte der Objektorientierung in PHP selbst ausprobiert werden.

## Der Startercode

Im Repository sind zwei Dateien bereits vorgegeben:

- `WET2UE04/Vehicle.php`: Diese Datei enthält noch eine leere Definition der Klasse `Vehicle`. Sie befindet sich im Namespace `WET2UE04` und ist daher auch im gleichnamigen Unterordner abgelegt.
- `index.php`: Die Hauptdatei. Sie wird vom Browser aufgerufen und legt im Verlauf der Übung Objekte wie `Vehicle` (und weitere) an.

In PHP ist es üblich, zwischen aufgerufenen und inkludierten Dateien zu unterscheiden. Erstere sind in der Regel kleingeschrieben, während Klassen, die nicht direkt aufgerufen werden sollen, großgeschrieben, d. h. in CamelCase.

Dateien, die aufgerufen werden, erzeugen auch die Ausgabe, während einzubindende Dateien lediglich Dinge definieren, d. h. beim Aufruf "weiß bleiben" (also keine Ausgabe erzeugen).

## Ablauf des Beispiels

Dieses Beispiel behandelt die wichtigsten objektorientierten Themengebiete:

### Klassen und Objekte

- Vervollständigt die Klasse `Vehicle` um Eigenschaften, einen Konstruktor und Methoden.
- Experimentiert mit den verschiedenen Sichtbarkeiten für Eigenschaften (`public`, `protected` und `private`).
- Legt zwei `Vehicle`-Objekte an.
- Greift auf die Eigenschaften der Objekte zu.
- Ruft die Methoden der Objekte auf.
- Legt eine statische Variable an und greift darauf zu.
- Legt eine statische Funktion an und ruft diese auf.
- Legt eine Konstante fest und greift auf sie zu.
- Legt eine readonly-Eigenschaft an und greift auf diese zu.

### Interfaces

- Definiert ein Interface `Movable` mit der Methode `move()`.
- Weist das Interface der Klasse `Vehicle` zu.
- Passt die Klasse `Vehicle` so an, dass sie dem Interface entspricht.
- Legt eine Klasse `Robot` an, die ebenfalls das Interface `Moveable` implementiert, und erzeugt ein Objekt davon.
- Erstellt ein Array mit den zwei `Vehicle`-Instanzen und der `Robot`-Instanz sowie Zahlen und String-Werten.
- Iteriert in einer Schleife über das Array und testet mit `instanceof`, ob es sich um eine `Movable`-Instanz handelt. Wenn ja, ruft `move()` auf.

### Vererbung und abstrakte Klassen

- Erzeugt eine Klasse `Car`, die von `Vehicle` erbt.
- Erstellt einen Konstruktor, der den Elternkonstruktor aufruft.
- Legt eine `Car`-Instanz an und gebt im Konstruktor die Eigenschaften von `Vehicle` aus, um die Auswirkungen der Sichtbarkeit zu testen.
- Optional: Macht `Vehicle` abstrakt und definiert eine abstrakte Methode `start()`.
- Optional: `Vehicle`-Objekte können nun nicht mehr instanziiert werden; zudem muss `Car` nun die abstrakte Methode implementieren.

### Traits und Enums

- Erstellt den Trait `Beep` mit der Methode `beep()`.
- Bindt den Trait in der Klasse `Robot` ein und ruft `beep()` beim `Robot`-Objekt auf.
- Macht dasselbe beim `Vehicle`-Objekt.
- Erstellt ein Enum `Color`, das die Farben "Red", "Green" und "Blue" enthält.
- Tauscht den Parameter `$color` bei `Vehicle` und `Vehicle` von `string` auf `Color` und passt die Aufrufe an.

## Tipps und Richtlinien

- Verwendet eine IDE, die für PHP konzipiert wurde (z. B. PhpStorm). Dies erleichtert euch Arbeit ungemein, wenn neue Klassen angelegt, Namespaces hinzugefügt oder Dateien eingebunden werden.
- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.