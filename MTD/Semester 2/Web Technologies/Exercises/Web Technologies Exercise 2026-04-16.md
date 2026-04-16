#web_technologies #php 

We’re doing php

# Übung 2 – Erste Schritte mit PHP  
  
WET2UE Web Technologies | Wolfgang Hochleitner | Code-Along  
  
Diese Einheit dient zum Erlernen der grundlegenden PHP-Konzepte. Es sollen die ersten einfachen Seiten erstellt und betrachtet werden. Es empfiehlt sich, dass jede*r im Code-Along mitarbeitet.  
  
## Repository clonen  
  
Clont im ersten Schritt das Repository eures GitHub-Classroom-Teams, damit ihr lokal (im Docker-Container) damit arbeiten könnt. Verwendet dazu PhpStorm ("Clone Repository/Project from Version Control...") oder einen Git-Client eurer Wahl.  
  
Clont das Repository in das `webapp`-Verzeichnis des Docker-Containers, sodass das Projekt im Verzeichnis `webapp/ue02-YourTeamName` zu liegen kommt.  
  
## Eigenen Branch erstellen  
  
Damit beide im Team während des Code-Alongs parallel arbeiten können, ohne sich gegenseitig Commits zu überschreiben, legt jede*r im Team einen eigenen Branch an. So kann jede*r unabhängig committen und pushen.  
  
In PhpStorm:  
  
1. Unten rechts in der Statusleiste auf die Branch-Anzeige (zeigt aktuell `main`) klicken.  
2. *New Branch* auswählen.  
3. Einen eindeutigen Namen vergeben (z.B. den eigenen Vornamen oder ein Kürzel).  
4. Mit *Create* bestätigen. PhpStorm wechselt direkt auf den neuen Branch.  
5. Beim ersten Push wird der Remote-Branch automatisch mit angelegt, wenn die entsprechende Option bestätigt wird.  
  
Ab diesem Zeitpunkt arbeitet jede Person im Team auf dem eigenen Branch. Committed und gepushed wird nach jedem Teil des Code-Alongs.  
  
## Die sechs Teile  
  
Während des Code-Alongs werden sechs PHP-Dateien mit Inhalt befüllt, die jeweils ein Thema aus der Vorlesung behandeln. Die Dateien liegen mit einem HTML-Grundgerüst im Repository – der PHP-Code wird dort ergänzt, wo der Kommentar `<!-- Hier kommt der PHP-Teil -->` steht. Nach jedem Teil kann die entsprechende Datei im Browser unter `http://localhost:8080/ue02-YourTeamName/<dateiname>.php` aufgerufen werden.  
  
### Teil 1 – `01-hello-world.php`  
  
Einbettung von PHP in HTML, Ausgabe mit `echo`, Short Echo Tags.  
  
- Im Body eine Zeile statisches HTML (z.B. einen `<p>`-Absatz) ergänzen.  
- Einen PHP-Block mit `<?php ... ?>` einfügen, in dem mit `echo` ein `<p>`-Absatz ausgegeben wird.  
- Einen weiteren PHP-Block mit mehreren `echo`-Anweisungen ergänzen.  
- Mit einem Short Echo Tag (`<?= ... ?>`) das aktuelle Datum mittels `date("d.m.Y")` und die Uhrzeit mittels `date("H:i")` ausgeben.  
  
### Teil 2 – `02-variables.php`  
  
Variablen, skalare Datentypen, implizite Typumwandlung, Vergleichsoperatoren `==` und `===`.  
  
- Variablen für die skalaren Datentypen anlegen (Integer, Float, String, Boolean, `null`) und mit `echo` ausgeben.  
- Mit `var_dump()` Typ und Wert mehrerer Variablen ausgeben.  
- Demonstrieren, dass Variablennamen Groß-/Kleinschreibung unterscheiden (`$wert` vs. `$Wert`).  
- Eine Variable mit einem Integer befüllen, dann mit einem String überschreiben (dynamische Typbindung).  
- Implizite Typumwandlung zeigen: einen numerischen String und einen Integer mit `+` addieren.  
- Einen Integer und einen String mit gleichem Wert anlegen und mit `==` und `===` vergleichen.  
- Explizites Casting mit `(int)` auf einen String anwenden.  
  
### Teil 3 – `03-operators.php`  
  
Arithmetische Operatoren, Stringkonkatenation mit `.`, Null Coalescing `??`, Spaceship-Operator `<=>`.  
  
- Einige arithmetische Berechnungen (`+`, `%`, `**`) in Variablen speichern und ausgeben.  
- Zwei Strings mit `.` zur Stringkonkatenation verbinden.  
- Demonstrieren, was passiert, wenn man Zahlen mit `.` konkateniert (`"10050"`) bzw. mit `+` addiert (`150`).  
- Die verkürzte Zuweisung `.=` zeigen.  
- Ternären Operator `? :` für eine Wenn-dann-Zuweisung verwenden.  
- Null Coalescing `??` mit einer nicht existierenden und einer existierenden Variable testen.  
- Null Coalescing Assignment `??=` zeigen.  
- Spaceship-Operator `<=>` mit drei Vergleichen demonstrieren (kleiner, gleich, größer).  
  
### Teil 4 – `04-control-structures.php`  
  
Verzweigungen mit `if`/`elseif`/`else`, `switch` und `match`, Schleifen `for` und `while`.  
  
- Eine `if`/`elseif`/`else`-Kaskade schreiben, die je nach Punktewert eine Note zuweist.  
- Eine `switch`-Anweisung schreiben, die abhängig von einem Ampel-Farbwert eine Aktion bestimmt.  
- Dieselbe Logik nochmals als `match`-Ausdruck umsetzen, um die kürzere und striktere Variante zu sehen.  
- Eine `for`-Schleife schreiben, die fünf `<li>`-Elemente in einer `<ul>` ausgibt.  
- Eine `while`-Schleife mit Countdown von 10 auf 0 implementieren.  
- Eine verschachtelte `for`-Schleife schreiben, die das kleine Einmaleins (1 bis 5) als HTML-Tabelle erzeugt.  
  
### Teil 5 – `05-strings.php`  
  
Einfache und doppelte Anführungszeichen, Variableninterpolation, wichtige (Multibyte-)Stringfunktionen.  
  
- Denselben String einmal in einfachen und einmal in doppelten Anführungszeichen ausgeben, um den Unterschied bei der Variableninterpolation zu sehen.  
- Curly-Syntax `{$variable}` verwenden, um eine Variable klar im String abzugrenzen.  
- Escape-Sequenzen `\n` und `\t` in einem doppelt gequoteten String testen.  
- Ein Wort mit Umlaut (z.B. „Hällo") mit `strlen()` und `mb_strlen()` messen, um den Unterschied zwischen Byte- und Zeichenzählung zu sehen.  
- Mit `mb_substr()` Teile eines Strings extrahieren (positive und negative Indizes testen).  
- Mit `trim()` führende und nachfolgende Leerzeichen aus einem String entfernen.  
- Mit `mb_strpos()` die Position eines Substrings in einem Satz finden.  
- Mit `mb_convert_case()` einen String einmal in Großbuchstaben und einmal in Title Case umwandeln.  
  
### Teil 6 – `06-greeting.php`  
  
Synthese der vorigen Teile mit Fokus auf *Advanced Escaping*, dem Verzahnen von HTML und PHP zur dynamischen Seitenerzeugung.  
  
- Vor dem `<!DOCTYPE html>` einen PHP-Block einfügen, in dem alle Daten (Name, aktuelle Stunde, Begrüßung per `match (true)`, gesäuberter Name mit `trim()` und `mb_convert_case()`, Namenslänge mit `mb_strlen()`) vorbereitet werden.  
- Im `<title>`, `<h1>` und im Uhrzeit-`<p>` die hardcoded Werte durch Short Echo Tags (`<?= ... ?>`) ersetzen.  
- Den Absatz „Dein Name ist 12 Zeichen lang. Ganz schön lang!" durch eine `if`/`else`-Verzweigung in **Doppelpunkt-Syntax** (`if (...): ... else: ... endif;`) ersetzen, die je nach Namenslänge einen anderen Kommentar ausgibt.  
- Die statische Liste der Buchstaben durch eine `for`-Schleife in **Doppelpunkt-Syntax** (`for (...): ... endfor;`) ersetzen, die den Namen mit `mb_substr()` zeichenweise zerlegt und für jedes Zeichen ein `<li>` ausgibt.  
  
## Tipps und Richtlinien  
  
- PHP-Dateien müssen immer über den Webserver aufgerufen werden (`http://localhost:8080/...`). Ein direktes Öffnen der Datei im Browser (`file:///...`) zeigt nur den Quellcode an, da keine PHP-Engine eingebunden ist.  
- Verwendet das [PHP Manual](https://www.php.net/manual/de/) zum Nachschlagen von PHP-Funktionen und Sprachkonstrukten.  
- Achtet beim Speichern der Dateien auf die Zeichenkodierung **UTF-8** (in PhpStorm unten rechts in der Statusleiste ersichtlich und einstellbar), damit Umlaute korrekt dargestellt werden.  
- Fehlermeldungen des PHP-Parsers enthalten in der Regel die Zeile und eine klare Beschreibung des Problems. Lest diese Meldungen genau, und fragt ggf. ein LLM um diese zu erklären.  
- Die Funktion `var_dump()` zeigt Typ und Wert einer Variable an und ist ein praktisches Debug-Werkzeug in PHP.  
- Committet und pusht nach jedem Teil auf euren eigenen Branch, damit der Fortschritt laufend gesichert wird.