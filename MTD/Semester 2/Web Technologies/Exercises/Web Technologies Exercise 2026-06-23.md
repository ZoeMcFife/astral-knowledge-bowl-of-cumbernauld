#web_technologies 

# Übung 12 – Slim Framework

WET2UE Web Technologies | 23.06.2026 | Wolfgang Hochleitner | Abgabe

Diese Übung erweitert den Slim-Skeleton um einen `POST /users`-Endpoint, sodass dieselbe REST-API entsteht, die in Übung 11 mit dem *fhooe/router-skeleton* gemeinsam implementiert wurde.
Der AJAX-Client aus Übung 11 kann nach Anpassung der Konfiguration in der `.env` wiederverwendet werden.

## Startercode lauffähig machen

1. Repository in den `webapp`-Ordner clonen.
2. Mit `docker exec -it webapp /bin/bash` in den Docker-Container wechseln.
3. `cd ue12-YourTeamName/slim-api && composer install`
4. Basispfad in `public/index.php` anpassen:
   ```php
   $app->setBasePath("/ue12-YourTeamName/slim-api/public");
   ```
5. `chmod -R 777 .` um Permissions anzupassen (falls nötig).
6. http://localhost:8080/ue12-YourTeamName/slim-api/users aufrufen: 5 lokal gespeicherte Beispiel-User werden im JSON-Format des Slim-Skeletons zurückgegeben.

## Code-Along (Beginn der Übung)

Gemeinsam werden folgende Schritte durchgeführt:

1. **Struktur ansehen**: `app/`, `src/Application/Actions/`, `src/Domain/`, `src/Infrastructure/`
2. `ListUsersAction` und `ViewUserAction` verstehen: ADR-Pattern, `__invoke()`, `respondWithData()` vs. `respondRaw()`
3. `InMemoryUserRepository` ansehen: Interface-Pattern, Dependency Injection, fertige Implementierung als Ausgangspunkt
4. `DbUserRepository` vollständig implementieren:
   - `findAll()`: `SELECT * FROM user`, alle Zeilen mit `fetchAll()` zurückgeben.
   - `findUserOfId()`: Parameterized Query, `fetch()`, `UserNotFoundException` bei nicht gefunden, `User`-Objekt erstellen
   - `userExists()`: `SELECT id FROM user WHERE username = :username`, `rowCount() > 0`
   - `addUser()`: `INSERT INTO user SET username = :username, name = :name, email = :email`
5. `app/repositories.php` auf `DbUserRepository` umstellen und auskommentierte Zeile aktivieren
6. Falls nötig, Datenbank importieren: phpMyAdmin → Importieren → `db/ue11_12_users.sql`
7. Testen: `GET /users` und `GET /users/1`

## Aufgabe

### Teil 1: `ListUsersAction` anpassen

In `src/Application/Actions/User/ListUsersAction.php` die Methode `respondWithData()` durch `respondRaw()` ersetzen:

```php
return $this->respondRaw($users);
```

**Warum?** Wie mit den ursprünglichen Daten ersichtlich, verpackt `respondWithData()` die Daten in ein `ActionPayload`-Objekt (`{"statusCode":200,"data":[...]}`). Der UE11-AJAX-Client benötigt jedoch eine andere (flachere) JSON-Struktur. `respondRaw()` gibt ein solches flaches JSON-Array zurück, genau wie der UE11-REST-Server.

Testen: `GET /users` muss danach ein JSON-Array in der Form `[{...}, ...]` ohne Felder wie den Statuscode liefern.

### Teil 2: `AddUserAction` implementieren

In `src/Application/Actions/User/AddUserAction.php` die Methode `action()` implementieren. Als Vorlage für die Logik kann Übung 11 dienen.

1. POST-Felder aus dem Request-Body lesen:
   ```php
   $data = (array) $this->request->getParsedBody();
   ```
   
   Die liefert den Body. Die darin enthaltenen Daten aus dem Formularfeld können dann aus `$data` mit den jeweiligen Schlüsseln abgefragt werden.
2. Guard-Clause: Leerer Username: Mit Hilfe von `respondRaw()` mit Status 400 und dem passenden `result` und der erklärenden `message` antworten.
3. Guard-Clause: Username existiert bereits: Mit Hilfe von `respondRaw()` mit Status 409 und dem passenden `result` und der erklärenden `message` antworten.
4. `addUser(...)` ausführen und mit Hilfe von `respondRaw()` mit Status 201 und dem passenden `result` und der erklärenden `message` antworten, wenn die Operation erfolgreich war. Ansonsten mit Status 500 einen Fehler anzeigen.

Alle Responses müssen in diesem Format (identisch zu Übung 11) erfolgen, damit der Client sie versteht:
```json
{
    "result": "User created",
    "message": "User tee343 successfully added."
}
```

Für alle Responses `respondRaw()` verwenden:
```php
return $this->respondRaw(['result' => '...', 'message' => '...'], 201);
```

## Optional: Mit UE11-AJAX-Client testen

1. Im UE11-AJAX-Client die `.env`-Datei anpassen:
   ```
   REST_API_URL=/ue12-YourTeamName/slim-api/public
   ```
2. Nach Erledigung beider Aufgaben oben sollten `GET /users` und `POST /users`
   im AJAX-Client identisch funktionieren wie mit dem UE11-REST-Server.

## Tipps und Richtlinien

- Bei Fragen oder Problemen zur Aufgabe eröffnet ein Issue in eurem Repository.