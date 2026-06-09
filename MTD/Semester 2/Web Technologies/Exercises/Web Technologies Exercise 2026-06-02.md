#web_technologies #php #time 
# Übung 9 – Internationalisierung  
  
WET2UE Web Technologies | 02.06.2026 | Wolfgang Hochleitner | Code-along  
  
Diese Übung legt den Fokus auf die Internationalisierung bzw. Lokalisierung einer PHP-Applikation. Dabei soll auf Basis eines `fhooe/router-skeleton` Projekts mit der Route `blog` gearbeitet werden. Für diese Route wird etwas Text (ein fiktiver Blogbeitrag) in einem Latte-Template angezeigt und mittels Translator-Komponente mehrsprachig gemacht.  
  
## Den Startercode zum Laufen bringen  
  
Zunächst muss der Startercode im Ordner `i18n` lauffähig gemacht werden. Es handelt sich dabei um ein auf *fhooe/router-skeleton* basierendes Projekt. Folgende Dinge sind zu tun.  
  
1. Basispfad in `public/index.php` in Zeile 49 anpassen.  
2. Dependencies installieren: zunächst eine Bash-Shell in den Docker-Container öffnen: `docker exec -it webapp /bin/bash`, dann in den Ordner des Projekts und den Unterordner `i18n` wechseln: `cd ue09-YourTeamName/i18n`. Nun mit `composer install` die Dependencies installieren. Dies erzeugt den `vendor`-Ordner.  
3. Projekt wie gewohnt aufrufen (zum Ordner `public` navigieren). Z.B. http://localhost:8080/ue09-YourTeamName/i18n/public/. Die Index-Route wird angezeigt.  
4. Sollte eine Fehlermeldung über fehlende Rechte zum Schreiben der Log-Datei angezeigt werden, so erteilt dem gesamten Ordner `ue09-YourTeamName` und allen Unterordnern mit folgendem Befehl entsprechende Rechte:  
   ```bash  
   chmod -R 777 .   ```  
## Code-Along: Erstellen eines einfachen, mehrsprachigen Blogartikels  
  
Es existiert bereits die Route `GET /blog/{locale}`, die das Template `blog.latte` anzeigt und einen beliebigen Text für die Sprache nach dem Slash akzeptiert. Wie auch bereits in den vergangenen Übungen demonstriert, ist es jedoch sinnvoll, eine eigene Klasse zu verwenden, um die Logik bei der Anzeige (hier die Übersetzung) zu kapseln.  
  
### Umstellen der Ausgabe auf die Klasse `Blog`  
  
1. Im Ordner `src` befindet sich bereits eine Klasse `Blog` im Namespace `WET2UE09` (der volle Pfad lautet `src/WET2UE09/Blog.php`). Der Konstruktor akzeptiert als Argumente einen String für die Locale und ein Latte-Objekt. Legt ein Objekt der Klasse `Blog` and und weist beim Anlegen den Platzhalter-Parameter `$locale` and `$this->locale` und das Latte-Objekt `$latte` an `$this->latte` zu.  
2. Editieren Sie `displayOutput()` in der Klasse `Blog` und zeigen Sie dort das Template `blog.html.twig` an.  
3. Ändert die Route für `GET /blog/{locale}` in `public/index.php` so, dass nun ein Objekt der Klasse `Blog` angelegt wird, welches das Latte-Objekt übergeben bekommt. Rufen Sie danach `displayOutput()` auf.  
4.  Die Applikation ist nun vorbereitet, um Elemente daraus mehrsprachig zu gestalten.  
  
### Die Übersetzung implementieren  
  
1. Installiert in der Shell eures Containers im `i18n`-Verzeichnis die Composer-Komponenten für die Übersetzung die YAML-Unterstützung:  
   ```bash  
   composer require symfony/translation symfony/yaml   ```2. Definiert eine Eigenschaft `$translator` vom Typ `Translator` (voller Namespace `Symfony\Component\Translation\Translator`) und legt im Konstruktor ein `Translator`-Objekt mit `$this->locale` als Argument an.  
2. Fügt dem Latte-Objekt, wie in der Vorlesung gezeigt, eine `TranslatorExtension` mit dem Translator-Objekt hinzu, um die Übersetzung direkt in den Templates zu ermöglichen.  
3. Legt einen Ordner `translations` an und erstellt darin eine Datei `messages+intl-icu.en.yaml` mit folgendem Inhalt:  
   ```yaml  
   ---  
   title: News from Web Technologies  
   ```5. Fügt dem `Translator`-Objekt einen `YamlFileLoader` und anschließend die YAML-Datei als Ressource hinzu.  
4. Passt das Template `blog.latte` an und fügt Folgendes an Stelle des Titels (innerhalb der `<h2>`-Überschrift) ein:  
   ```latte  
   {_'title'}  
   ```7. Betrachtet die Webseite. Es sollte sich nichts verändert haben, jedoch wird der Titel bereits aus der Übersetzungsdatei entnommen und angezeigt.  
5. Kopiert nun `messages+intl-icu.en.yaml` zu `messages+intl-icu.de.yaml` und passt den String darin auf "Neues aus Web Technologies" an. Fügt die Datei als Ressource beim Translator hinzu.  
6. Ändert den URL hinten auf "de". Der Titel sollte nun in Deutsch angezeigt werden.  
  
### Die Locales absichern  
  
Der Sprachumschalter funktioniert ganz einfach aufgrund des URL-Platzhalters. Was passiert aber, wenn jemand eine Sprache dort hineinschreibt, für die keine Übersetzungen vorhanden sind, oder einen String, der keine Locale-Angabe ist?  
  
Der Enum `BlogLocale` managed die verfügbaren Sprachen. Zwei Cases definieren ENGLISCH und DEUTSCH. Der Enum ist ein sogenannter Backed Enum, d. h. hinter jedem Case ist ein String-Wert ("en" und "de") hinterlegt.  
  
- Die Methode `default()` gibt den Standardwert (hier `"en"`) zurück.  
- Die Methode `values()` gibt die unterstützten Sprachen als String-Array (hier `["en", "de"]`) zurück.  
- Die Methode `verify()` erlaubt es mithilfe der PHP-Klasse `Locale` eine angegebene Locale zu verifizieren. Die Methode `lookup()` gleicht dabei eine übergebene Locale mit einer Liste von hinterlegten ab und gibt sie zurück. Ist eine Locale nicht vorhanden, wird ein Fallback zurückgegeben (hier "en").  
  
1. Um die Locales zu verifizieren, ändert im Konstruktor der Klasse `Blog` das Setzen der Locale:  
   ```php  
   $this->locale = BlogLocale::verify($locale);  
   ```  
   Nun kann jeder beliebige Wert eingegeben werden. Wird er nicht gefunden, wird Englisch verwendet, werden aber etwa Locales wie "de-AT" angegeben, wird die "de"-Locale ausgewählt.  
  
### Weitere Elemente in die Übersetzung inkludieren  
  
Übersetzen Sie noch weitere Elemente im Blog-Header:  
  
1. "Written by The Autor": Erstellt einen Eintrag `author` im YAML-File und arbeitt mit dem ICU MessageFormat. Verwendet z.B. `{ name }` als Platzhalter für den Autor\*innennamen. Im Template müsst ihr diesen über `{_'author', ['name' => $author_name]}` einfügen. Den Parameter `author_name` (Name frei wählbar) gebt ihr beim `render()`-Aufruf des Templates mit. So steuert ihr dynamisch von PHP, welcher Name eingefügt wird.  
2. "Datum": Erstellen Sie einen weiteren Eintrag in der YAML-Datei mit einem ICU Datums-String, damit dieses der jeweiligen Locale angepasst wird. Das Datum wird von PHP aus als `DateTime`-Objekt übergeben.  
3. "Reading time: 1 minute": Erstellet einen ICU `plural` String, mit dem ihr die korrekte Pluralform von "Minute" angebt und darüber hinaus die Minuten als Parameter übergeben könnt.  
4. Probiert, wie sich diese 3 einzeln übersetzen Elemente mithilfe des ICU MessageFormats in einem String zusammenfassen lassen.  
  
Abschließende Anmerkung: Der Blogbeitrag selbst würde nicht in den YAML-Dateien gespeichert und übersetzt. Dieser kommt in der Regel direkt aus der Datenbank, wo er in mehreren Sprachen separat (in eigenen Tabellen oder Spalten einer Tabelle) hinterlegt wird.  
  
## Tipps und Richtlinien  
  
- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.