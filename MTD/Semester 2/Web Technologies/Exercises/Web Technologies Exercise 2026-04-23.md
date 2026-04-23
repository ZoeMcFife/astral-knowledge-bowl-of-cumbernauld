#web_technologies #php 

WET2UE Web Technologies | 23.04.2026 | Wolfgang Hochleitner | Abgabe

Die dritte Übung setzt zum ersten Mal eine einfache Webapplikation mit Request und Response um. In ein Formular wird ein Text eingegeben, der an mehreren Stellen Geldbeträge enthält. Manche in korrekten Formaten, manche in inkorrekten. Nach dem Absenden des Formulars sollen die korrekt formatierten Geldbeträge mithilfe regulärer Ausdrücke (Regular Expressions, Regex) erkannt und summiert werden. Die Summe wird zusammen mit dem eingegebenen Text angezeigt.

## Repository clonen

Clont im ersten Schritt das Repository eures GitHub-Classroom-Teams, damit ihr lokal (im Docker-Container) damit arbeiten könnt. Verwendet dazu PhpStorm ("Clone Repository/Project from Version Control...") oder einen Git-Client eurer Wahl.

Clont das Repository in das `webapp`-Verzeichnis des Docker-Containers, sodass das Projekt im Verzeichnis `webapp/ue02-YourTeamName` zu liegen kommt.

Ihr arbeitet in dieser Übung gemeinsam am Projekt, d. h. gemeinsam im `main`-Branch. Falls ihr möchtet, könnt ihr mit Branches arbeiten, d. h. eine Person legt sich einen Branch an und hat somit eine separate Codebasis, die zu einem späteren Zeitpunkt in den `main`-Branch gemerged werden kann. Für den Anfang ist es vermutlich einfacher, sich abzustimmen, wer am Beispiel arbeitet, um Merge-Konflikte zu vermeiden. Hat eine Person committed und den Code auf GitHub gepusht, so muss die zweite zunächst einen Pull ausführen, um die aktuelle Version zu erhalten.

## Geld zählen mit Regular Expressions

Ihr findet in eurem Repository drei Dateien, die zum Übungsbeispiel gehören:

- `index.html`: Die Startseite, auf der ihr das Formular zur Texteingabe erstellt und die PHP-Seite zur Verarbeitung aufruft.
- `moneycounter.php`: Die PHP-Datei wird nach Absenden des Formulars aufgerufen und erhält den eingegebenen Text. Dort wird die Regular Expression angewandt, um die Geldbeträge herauszufiltern, zu summieren und auszugeben.
- `teststring.txt`: Enthält die Zeichenkette, mit der getestet werden soll. Die darin enthaltenen korrekten Geldbeträge ergeben genau 1.000 EUR.

### Die HTML-Datei (`index.html`)

Die HTML-Datei enthält bereits ein grobes HTML-Grundgerüst mit einem Container, Überschriften und etwas Text. Erstellt darin nun ein Formular mit einer `<textarea>`, die mehrzeilige Eingaben erlaubt. Das Eingabefeld soll ein Label und einen Submit-Button zum Absenden enthalten.

Das Formular soll mit der Methode `POST` übermittelt werden und die PHP-Datei `moneycounter.php` aufrufen. In ihr steht dann der eingegebene Text zur Verarbeitung bereit.

> [!TIP]
> Fügt den zu analysierenden Text aus `teststring.txt` zwischen `<textarea>` und `</textarea>` ein. Auf diese Weise ist das Eingabefeld bereits vorausgefüllt und ihr könnt schneller testen.

Wenn ihr wollt, könnt ihr das Formularfeld mit Bootstrap-Klassen formatieren. Das Framework ist bereits eingebunden und stellt Klassen für Formularfelder zur Verfügung: `form-control` für Eingabefelder, `form-label` für Labels und `btn btn-primary` für einen Button.

Die folgende Abbildung zeigt das Formular mit Text vor dem Absenden.

| <img src="form.png" alt="Screenshot eins Formulars mit Text und der Schaltfläche 'Count Money'." width="800">  |
|:--------------------------------------------------------------------------------------------------------------:|
| **Im Formular wird der Teststring eingetragen. Mit einem Klick auf den Button wird die PHP-Datei aufgerufen.** |

### Die PHP-Datei (`moneycounter.php`)

Die PHP-Datei wird vom HTML-Formular aufgerufen und erhält den eingegebenen Text im superglobalen Array `$_POST`. Fragt diesen Text ab und speichert ihn in einer Variable.

Erstellt euch nun eine Variable, die die Summe der Geldbeträge enthält.

Schreibt nun eine Regular Expression, die alle gültigen Geldbeträge findet (Abschnitt [Gültige Formate](#gültige-formate)). Wendt den regulären Ausdruck mit `preg_match_all()` an, damit ihr alle Vorkommen im Text zurückgeliefert bekommt. Setzt in eurer Regex so Klammern, dass ihr die Beträge aus dem `$matches`-Array (dritter Parameter von `preg_match_all()`) auslesen könnt.

Summiert nun die gefundenen, korrekten Beträge und gebt die Summe mit `echo` aus. Formatiert die Zahl mit `number_format()` so, dass zwei Nachkommastellen angezeigt werden, als Dezimaltrennzeichen der Punkt (`.`) und als Tausendertrennzeichen ein Komma (`,`) angezeigt werden.

Gebt weiters den Originaltext noch aus. Die folgende Abbildung zeigt das Ergebnis.

| <img src="result.png" alt="Screenshot mit Ausgabe eines Textes und eines gezählten Geldbetrages." width="800">  |
|:---------------------------------------------------------------------------------------------------------------:|
| **Nachdem das Formular abgeschickt wurde, werden auf der PHP-Seite der Originaltext und die Summe ausgegeben.** |

### Gültige Formate

Gültige Geldbeträge sind Dezimalzahlen ohne oder mit genau zwei Nachkommastellen oder einem Bindestrich. Das Dezimaltrennzeichen ist der Punkt (`.`). Die Zahl vor dem Dezimaltrennzeichen kann beliebig groß sein (von 0 weg ist alles möglich). Nach dem Dezimaltrennzeichen darf nur eine zweistellige Zahl oder ein Bindestrich stehen. Als Währungssymbol soll nur „EUR“ in genau dieser Schreibweise erkannt werden. Geldbeträge können am Anfang und Ende der Zeichenkette sowie in der Mitte stehen. In der Mitte muss ein Abstand (Leerzeichen) zu den vorigen und nachfolgenden Zeichen gegeben sein.

Beispiele für korrekte Formate:

- 12 EUR
- 123.- EUR
- 3.50 EUR

### Ungültige Formate

- .50 EUR (keine Zahl vor dem Dezimaltrennzeichen)
- 12.5 EUR (nur eine Nachkommastelle)
- 13.1743 EUR (zu viele Nachkommastellen)
- 24.50EUR (kein Leerzeichen zwischen Betrag und EUR)
- 2,70 EUR (Komma statt Punkt als Dezimaltrennzeichen)
- 12,50 eur (EUR kleingeschrieben)

## Beispielprojekt: Datumsumwandlung

Im Verzeichnis `examples` eures Repositorys findet ihr ein Beispiel zur Datumsumwandlung. Ihr könnt dieses Beispiel ausprobieren und analysieren, um zu sehen, wie mithilfe regulärer Ausdrücke und der Funktion `preg_match()` ein Muster in einer Zeichenkette gesucht wird und das Ergebnis verwendet wird. Öffnet dazu `index.html` in eurem Docker-Container: http://localhost:8080/ue03-YourTeamName/examples/index.html (ihr müsst `ue03-YourTeamName` durch den Namen eures Teams auf GitHub ersetzen, damit der Link funktioniert).

In der Datei `README.md` im Verzeichnis `examples` findet ihr eine ausführliche Erklärung des Beispiels.

## Tipps und Richtlinien

- [regex101](https://regex101.com/) und [PHP Live Regex](https://www.phpliveregex.com/) sind praktische Seiten, um regular Expressions bzw. deren Ergebnisse schnell zu testen.
- Achtet darauf, dass ihr korrektes HTML erzeugt. Validiert den entstandenen Quellcode mit dem W3C-Validator.
- Verwendet das [PHP-Manual](https://www.php.net/manual/de/) zum Nachschlagen von PHP-Funktionen.
- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.