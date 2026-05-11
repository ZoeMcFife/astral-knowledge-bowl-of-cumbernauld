#web_technologies #php 

# Übung 6 – Routing und Templates  
  
WET2UE Web Technologies | 11.05.2026 | Wolfgang Hochleitner | Abgabe  
  
In dieser Übung geht es um die Erstellung einer Applikation mit `fhooe/router` und Latte-Templates. Wir werden gemeinsam die Applikation mit dem Router erstellen. Die Installation und Konfiguration der Template-Engine sowie die Aufgabe von zwei einfachen Templates sind dann Teil der Abgabe.  
  
## Vorbereitung  
  
Dieses Repository enthält keinen Startercode. Wir beginnen bei null, um gemeinsam ein neues Projekt aufzusetzen. Führt die folgenden Dinge zur Vorbereitung aus:  
  
- Clont beide im Team das Repository in das `webapp`-Verzeichnis eurer *fhooe-web-dock*-Instanz, sodass dort das Verzeichnis `ue06-YourTeamName` entsteht.  
- Öffnet eine Konsole: PowerShell, Terminal oder auch die in PhpStorm eingebaute Variante. Startet darin eine Bash-Shell im `webapp`-Container: `docker exec -it webapp /bin/bash`.  
  
## Ein Projekt mit `fhooe/router` anlegen (gemeinsam)  
  
1. Legt für das Projekt ein Verzeichnis `team-portal` an.  
2. Sucht auf [Packagist](https://packagist.org/) nach der `fhooe/router`-Bibliothek und bindet diese ein. Öffnet dazu wie oben beschrieben eine Bash-Shell im `webapp`-Container, wechselt in den Ordner `ue06-YourTeamName/team-portal` und führet das Composer Installations-Commando aus: `composer require fhooe/router`.  
3. Erstellt ein Unterverzeichnis `public` in `team-portal` und legt eine Datei `index.php` darin an.  
4. Erstellt ebenfalls in `public` eine Datei namens `.htaccess` und fügt folgenden Inhalt darin ein:  
   ```ApacheConf  
   <IfModule mod_rewrite.c>  
     RewriteEngine On        # Some hosts may require you to use the `RewriteBase` directive.  
     # Determine the RewriteBase automatically and set it as environment variable.     # If you are using Apache aliases to do mass virtual hosting or installed the     # project in a subdirectory, the base path will be prepended to allow proper     # resolution of the index.php file and to redirect to the correct URI. It will     # work in environments without path prefix as well, providing a safe, one-size     # fits all solution. But as you do not need it in this case, you can comment     # the following 2 lines to eliminate the overhead.     RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$     RewriteRule ^(.*) - [E=BASE:%1]        # If the above doesn't work you might need to set the `RewriteBase` directive manually, it should be the  
     # absolute physical path to the directory that contains this htaccess file.     # RewriteBase /        RewriteCond %{REQUEST_FILENAME} !-f  
     RewriteRule ^ index.php [QSA,L]   </IfModule>  
   ```5. Bindet nun in `index.php` den Composer Autoloader ein und legt ein neues `Router`-Objekt an.  
5. Definiert nun eure erste Route, nämlich `"/"`, also die Root-Route, die beim Aufruf des `public`-Verzeichnisses angezeigt wird. Gebt mit `echo` etwa ein "Hello World" aus. Die Methode `get()` des Routers ist hierfür die richtige.  
6. Startet nun das Routing, indem ihr die `run()`-Methode des Routers aufruft.  
7. Testet nun eure Route, indem ihr im Browser http://localhost:8080/ue06-YourTeamName/team-portal/public aufruft. Ihr bekommt eine Exception, da der 404-Handler noch nicht gesetzt ist. Dieser muss aber existieren, falls keine hinterlegte Route der aufgerufenen entspricht.  
8. Definiert den 404-Handler mit `set404Callback()`.  
9. Ruft die Route erneut auf. Es erscheint keine Fehlermeldung, allerdings wird der 404-Handler aufgerufen. Übergebt mit `use ($router)` das Router-Objekt in ihre 404-Callback Methode und lasst euch mit der `getUri()`-Methode den Pfad anzeigen. Der Router sieht den gesamten Pfad, wir möchten aber nur alles nach `public` tatsächlich berücksichtigen (der gesamte Pfad davor bleibt ja immer gleich).  
10. Setzt mit `$router->basePath` den Pfad "/ue06-YourTeamName/team-portal/public" als Basispfad, damit wird in der Route als gegeben angenommen und nicht mehr berücksichtigt.  
11. Ruft die Route erneut auf: Hello World sollte nun angezeigt werden.  
12. Definiert eine zweite Route `/other` und gebt anderen Text aus, um noch einen Pfad zu testen. Gebt ihr etwas anderes im Browser ein, sollte der 404-Handler aufgerufen werden.  
  
## Abgabe: Ein Team-Profil mit Latte-Templates erzeugen  
  
### Latte Installieren und konfigurieren  
  
1. Sucht auf [Packagist](https://packagist.org/) nach dem Paket `latte/latte` und installiert dieses mit Composer. Achtet darauf, dass ihr dies im Verzeichnis `team-portal` ausführt.  
2. Um in den Template-Dateien später URLs auf Routen, z.B. `/team` setzen zu können, die dann zum korrekten vollen Pfad aufgelöst werden, braucht ihr das Paket `fhooe/latte-extensions`. Es beinhaltet eine Latte-Extension namens `RouterExtension`, die zu Latte eine Funktion `{url_for()}` hinzufügt, die genau dies tut. Sucht nach dem Paket auf Packagist und macht euch mit der Einbindung und Funktionsweise vertraut.  
3. Erstellt nun ein Unterverzeichnis `views` in `team-portal`. Dort kommen später die Template-Dateien (`.latte`-Files) hinein.  
4. Initialisiert nun Latte in `index.php`, setzt das Cache- und Template-Verzeichnis und registriert die Router-Extension. Das Vorlesungsskript bzw. die Slides sind ein guter Anhaltspunkt. Registriert auch die `RouterExtension` als Erweiterung beim Latte-Objekt.  
  
### Die Startseite (`GET /`)  
  
1. Erstellt ein Template `index.latte` im Verzeichnis `views`.  
2. Die Seite soll eine vollständige HTML-Datei sein und eine Begrüßung und einen Link zur Route `/team` beinhalten. Hier braucht ihr `{url_for()}`.  
3. Passt die Route für `/` an: Entfernt das Hello World und rendert stattdessen `index.latte`.  
  
### Das Team-Profil (`GET /team`)  
  
1. Erstellt eine weitere Route für `GET /team`.  
2. Erstellt im Callback dieser Route ein assoziatives Array mit Daten für euer Team:  
   3. `teamName` (String),  
   4. `members` (Arrays mit den Namen der einzelnen Teammitglieder),  
   5. `motto` (String),  
   6. `isWorking` (Boolean)  
7. Rendert nun in dieser Route ein Template namens `team.latte` und übergebt dieses Array.  
8. Erstellt nun in `views` die Datei `team.latte` und gebt dort die Daten aus dem Array wie folgt aus:  
   9. Teamnamen komplett in Großbuchstaben (dafür einen Latte-Filter verwenden).  
   10. Eine ungeordnete Liste (`<ul>`) aller Teammitglieder, deren `<li>`-Elemente mit einer foreach-Schleife (mit Tags oder n:attributes) erstellt werden.  
   11. Motto ausgeben.  
   12. Mit einer if-Bedingung (als Tag oder mit n:attributes) unterschiedlichen Text ausgeben, je nachdem ob `isWorking` true oder false ist.  
13. Erstellt weiters wieder einen Link der zurück auf `/` verweist.  
  
## Tipps und Richtlinien  
  
- Verwendet eine IDE, die für die Verwendung mit PHP konzipiert wurde (z. B. PhpStorm). Benützt immer die Autovervollständigung, wenn ihr neue Objekte anlegt, damit der volle Namespace verwendet wird.  
- Für Syntax-Highlighting und Code Completion von Latte-Templates in PhpStorm empfiehlt sich das Plugin [Latte Support](https://plugins.jetbrains.com/plugin/24218-latte-support).  
- Falls Latte keine Schreibrechte hat, um die kompilierten Templates zu schreiben, führt Folgendes im `ue06-YourTeamName`-Verzeichnis aus: `chmod -R 777 .`. Dies setzt, ausgehend vom aktuellen Verzeichnis die Berechtigungen rekursiv in allen Unterverzeichnissen auf 777 (alle dürfen alles).  
- Achtet bei der Abgabe darauf, dass alle Files committed und gepushed sind. Vor allem `composer.json` und `composer.lock`, aber auch alle Templates und PHP-Datein. Beim Committen aufpassen, dass auch neue ("unversioned") Files ausgewählt sind.  
- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.