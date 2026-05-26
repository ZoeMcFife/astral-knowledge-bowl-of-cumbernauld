#web_technologies  #php 


# Übung 8 – Login

WET2UE Web Technologies | 26.05.2026 | Wolfgang Hochleitner | Abgabe

Übung 8 setzt dort an, wo Übung 7 aufgehört hat. Aus dem im Ordner `login` befindlichen Grundgerüst soll eine fertige, geroutete Applikation erstellt werden, die es ermöglicht, die Route "GET /main" nur zu betreten, wenn zuvor ein erfolgreicher Login mit User-Daten aus der Datenbank "ue07_08_login" erfolgt ist.

## Den Startercode zum Laufen bringen

Zunächst muss der Startercode im Ordner `login` lauffähig gemacht werden. Es handelt sich dabei um ein auf *fhooe/router-skeleton* basierendes Projekt. Folgende Dinge sind zu tun.

1. Den Basispfad in `public/index.php` in Zeile 59 anpassen.
2. Projekt wie gewohnt aufrufen (zum Ordner `public` navigieren). Kommt die Übersichtsseite, funktioniert alles. Kommt eine Fehlermeldung, sind die Dependencies noch nicht installiert. Mittels `docker exec -it webapp /bin/bash` eine Shell im Docker-Container öffnen, mit `cd ue08-YourTeamName/login` in den `login`-Ordner wechseln und mit `composer install` die Dependencies installieren.
3. Optional: Erscheint ein Fehler aufgrund fehlender Schreibrechte (meist unter Windows), dann im Ordner `login` ein `chmod -R 777 .` ausführen, um die Rechte im Verzeichnis und in allen Unterverzeichnissen zu setzen.
4. Optional: Datenbank und User anlegen. Sind von Übung 7 das Datenbankschema "ue07_08_login" mit der Tabelle "User" und beliebige Accounts vorhanden, können diese verwendet werden oder mit der `register`-Anwendung von Übung 7 auch neue angelegt werden. Alternativ können mit der Route `/createdb` (bereits fertig implementiert) zwei Accounts "user1@email.com" und "user2@email.com", jeweils mit Passwort "geheim" angelegt werden. Fehlen Schema oder Tabelle, werden diese ebenfalls erzeugt.

## Die Routen anlegen und die Session starten/fortsetzen

In der Datei `public/index.php` müssen nun die notwendigen Routen angelegt werden (`/` und `/createdb` existieren bereits).

1. `GET /login`: Zeigt das Template `login.latte` an, das das Loginformular enthält. Diese Route soll nur aufrufbar sein, wenn man *nicht* eingeloggt ist (mithilfe von `requireNotLoggedIn()` in `RouteGuard` testbar).
2. `POST /login`: Verarbeitet den Loginprozess. Dies geschieht in der Klasse `Login`. Von ihr soll hier ein Objekt angelegt werden, darauf `isValid()` und im Anschluss `displayOutput()` aufgerufen werden.
3. `GET /main`: Die geschützte Hauptseite. Die Route zeigt `main.latte` an. Dies darf jedoch nur erfolgen, wenn man eingeloggt ist (mithilfe von `requireLoggedIn()` in `RouteGuard` testbar).
4. `GET /logout`: Führt den Logout-Vorgang durch. Dies geschieht durch Leeren des Session-Arrays, Löschen des Session-Cookies und Beenden der Session. Danach wird auf `/` weitergeleitet.
5. Ebenso muss in Zeile 22 der `session_start()`-Block einkommentiert werden, um eine Session zu starten. Dabei werden Parameter mitgegeben, die das Session-Cookie sicherer machen.

## Die Klasse `Login` implementieren

Diese Klasse übernimmt die Abarbeitung des Login-Prozesses. Zunächst werden die Eingaben im Loginformular validiert. Bei gültigen Daten wird der Login verarbeitet, d. h. ein Login-Token wird gesetzt und auf die geschützte Seite weitergeleitet. Bei fehlerhaften Eingaben wird das Loginformular erneut angezeigt.

### Überprüfen der Logindaten

Die Eingaben des Loginformulars werden in der Methode `isValid()` überprüft. Folgende Dinge sind dabei zu tun:

1. Überprüfung der Eingaben auf formale Korrektheit. Schlägt eine Überprüfung fehl, wird eine Fehlermeldung im Array `$this->messages` gespeichert. Die statischen Methoden aus der Klasse `Utilities` können hierfür verwendet werden:
   2. Die E-Mail-Adresse darf nicht leer sein.
   3. Die E-Mail-Adresse muss in einem gültigen Format vorliegen.
   4. Das Passwort darf nicht leer sein.
5. Nun wird überprüft, ob diese Checks erfolgreich waren, d. h. ob das Array mit den Fehlermeldungen leer ist. Ist dies der Fall, wird geprüft, ob die Logindaten (E-Mail-Adresse und Passwort) vorhanden und gültig sind. Dies geschieht durch Aufruf von `authenticateUser()`. Diese Methode gibt `true` zurück, wenn die E-Mail-Adresse in der Datenbank vorhanden ist und das gespeicherte Passwort übereinstimmt. Ansonsten wird `false` zurückgegeben.
6. Je nachdem, welches Ergebnis von `authenticateUser()` zurückgegeben wurde, wird nun weiter verfahren:
   7. Bei `true`: Der Login war erfolgreich, es wird `business()` aufgerufen und der Login wird verarbeitet.
   8. Bei `false`: Es wird nichts mehr gemacht. Nach `isValid()` kommt ja `displayOutput()` (in der `POST /login`-Route) und diese Methode zeigt das Loginformular wieder an (um erneut Eingaben zu machen bzw. diese zu korrigieren).

### Den Login verarbeiten

Ein erfolgreicher Login wird in der Methode `business()` verarbeitet. Hier passieren die folgenden Dinge:

1. Die E-Mail-Adresse wird in der Session (im Schlüssel "email") gespeichert. Damit steht sie auf den weiteren Seiten zur Verfügung und kann etwa auf der geschützten Seite angezeigt werden, um darzustellen, dass man eingeloggt ist).
2. Es wird ein Login-Token generiert. Dazu steht in der Klasse `Utilities` die Methode `generateLoginToken()` zur Verfügung. Dieses Token wird ebenfalls in der Session, jedoch unter dem Schlüssel "loginToken" gespeichert. Dieser Wert bestimmt letztlich, ob man eingeloggt ist oder nicht.
3. Mithilfe des `Router`-Objekts wird nun zur Route `/main` weitergeleitet.

### Userdaten mit der Datenbank vergleichen

Die Methode `authenticateUser()` überprüft, ob die eingegebenen Daten (E-Mail-Adresse und Passwort) in der Datenbank genauso vorhanden sind.

1. Mithilfe eines SELECT-Statements wird ein Datensatz mit der im Loginformular eingegebenen E-Mail-Adresse gesucht.
2. Ist ein solcher vorhanden, wird mit der PHP-Funktion `password_verify()` überprüft, ob der beim Eintrag gespeicherte Passwort-Hash dem aktuell eingegebenen Passwort entspricht. Wenn ja, sind die Daten korrekt und es wird `true` zurückgegeben.
3. Bevor jedoch `true` zurückgegeben wird, soll mit `password_needs_rehash()` noch überprüft werden, ob der gespeicherte Passwort-Hash noch sicher ist. Gibt diese Methode `true` zurück, wird ein neues Passwort mit `password_hash` und `PASSWORD_DEFAULT` algorithmus erzeugt und mittels `updateUser()` in der Datenbank aktualisiert.
4. Ist entweder die E-Mail-Adresse gar nicht vorhanden oder das Passwort falsch, wird `false` zurückgegeben.

### Userdaten updaten

Im Falle eines zu aktualisierenden Passworts muss es in die Datenbank eingetragen werden. Dies geschieht in `updateUser($userID, $password)`.

1. Mit einem UPDATE-Statement wird beim Eintrag mit der übergebenen ID das übergebene Passwort gesetzt. Dies muss bereits gehasht übergeben werden (die Methode schreibt den String in die Datenbank).

### Erneute Anzeige des Loginformulars

Die Methode `displayOutput()` wird immer dann aufgerufen, wenn während des Login-Prozesses Fehler aufgetreten sind. Sie zeigt also das Loginformular wieder an, übergibt aber Fehlermeldungen und auch die E-Mail-Adresse, die bereits wieder eingetragen ist, wenn das Formular angezeigt wird.

1. Mit Latte das Template `login.latte` anzeigen.
2. Den Parameter "email" übergeben und dort die E-Mail-Adresse aus den POST-Daten mitgeben.
3. Den Parameter "messages" übergeben und dabei den Inhalt von `$this->messages` übergeben.

## Tipps und Richtlinien

- Verwendet eine IDE, die für die Verwendung mit PHP konzipiert ist (z. B. PhpStorm). Verwendet immer die Autovervollständigung, wenn ihr neue Objekte anlegt, damit der volle Namespace verwendet wird.

- Ihr könnt in PhpStorm eine Verbindung zu eurer Datenbank "ue07_08_login" herstellen, um diese direkt in der IDE inspizieren zu können. Öffnet dazu das Tool Window "Database" (rechts oben) und legt mit "+" eine neue Data Source an. Wählt MariaDB aus und gebt die folgenden Verbindungsparameter ein:

  - Host: localhost
  - Port: 6033
  - User: dbuser
  - Password: geheim
  - Database: ue07_08_login

  Die Verbindungsparameter sind hier unterschiedlich von denen in der Webapplikation. Denn im Beispiel verbindet ihr euch vom webapp-Container in den db-Container. Hier verbindet ihr euch von eurem Host-System in den db-Container (daher localhost und Port 6033 anstatt db und 3306).

- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.
