#web_fundementals #javascript 

wooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo


javascropt

# Übung 10 – JavaScript Teil 3: DOM & Form Validation
WEF1UE Web Fundamentals | 20.01.2026 | Wolfgang Hochleitner | Abgabe

Zum Kennenlernen des Document Object Models (DOM) soll eine Formularfeldüberprüfung an einem vorgegebenen Formular durchgeführt werden. Dabei kommen DOM-Methoden, EventListener und CSS-Klassenmanipulation zum Einsatz.

## Formularfeldüberprüfung

In Ihrem Repository finden Sie das Grundgerüst zu dieser Aufgabe, bestehend aus der HTML-Seite `index.html` und der JavaScript-Datei `js/main.js`. Das Projekt enthält ein einfaches Registrierungsformular, in dem Benutzer*innenname, E-Mail-Adresse, Adresse, Passwort sowie eine Auswahl an Hardware angegeben werden können.

Beim Absenden des Formulars wird in einem `submit`-Event-Listener die Funktion `isValid()` in der Datei `main.js` aufgerufen. Im Grundgerüst ist die Validierung teilweise vorbereitet, aber noch nicht funktionsfähig.

Vervollständigen Sie die JavaScript-Datei bzw. die darin enthaltenen Funktionen, sodass diese ihren Zweck erfüllen. Für das Styling von Fehlern werden die Standardmechanismen von Bootstrap verwendet.

### `isValid()`

Ergänzen Sie in dieser Funktion weitere Validierungschecks nach den folgenden Kriterien und rufen Sie im Fehlerfall die entsprechenden Hilfsfunktionen auf. Sind zwei Kriterien angegeben, sind auch getrennte Fehlermeldungen für jeden Fehlerfall auszugeben.

> [!IMPORTANT]
> Speichern Sie sich Referenzen zu den Eingabefeldern in Variablen (z.B. `const usernameInput = document.getElementById("username")`), um diese effizient an die Hilfsfunktionen weitergeben zu können.

#### Benutzer*innenname (ID `username`)

- Pflichtfeld: Darf nach dem Entfernen von Leerzeichen (`trim()`) nicht leer sein.

#### E-Mail-Adresse (ID `email`)

- Pflichtfeld: Darf nach dem Entfernen von Leerzeichen nicht leer sein.
- Format: Muss ein `@`-Zeichen enthalten (nutzen Sie `.includes("@")`).

#### Postleitzahl (ID `zip`)

- Optionales Feld: Darf leer bleiben.
- **Wenn etwas eingetragen wurde:**
    - Sind nur Zahlen erlaubt (keine Buchstaben oder Sonderzeichen).
    - Muss die Eingabe exakt 4 Stellen lang sein.

#### Passwort (ID `password1`)

- Pflichtfeld: Darf nicht leer sein. **Hinweis:** Passwörter werden *nicht* getrimmt (Leerzeichen sind gültige Zeichen).
- Länge: Muss mindestens 5 Zeichen lang sein.

#### Passwort-Wiederholung (ID `password2`)

- Pflichtfeld: Darf nicht leer sein.
- Übereinstimmung: Muss exakt mit dem ersten Passwort übereinstimmen.

#### Hardware-Checkboxen (Name `hardware`)

- Mindestens eine der fünf Checkboxen muss ausgewählt sein.

### `setErrorMessage(inputElement, message)`

Diese Funktion erzeugt dynamisch eine Fehlermeldung unter einem Eingabefeld. Anders als in früheren Übungen übergeben wir hier **nicht die ID als String**, sondern direkt das **HTML-Element** (den DOM-Knoten).

Diese Funktion übernimmt die Ausgabe der Fehlermeldungen. Sie wird aus `isValid()` heraus aufgerufen, wenn ein Kriterium nicht erfüllt ist (z. B. wenn das Feld nicht ausgefüllt ist). Die Funktion erzeugt ein `<div>`-Element mit der Fehlermeldung, die im Parameter `message` übergeben wird. Dieses Element wird unterhalb des übergebenen `<input>`-Felds angezeigt.

### `setCheckboxErrorMessage()`

Diese Funktion handhabt den Fehlerfall, wenn keine Hardware-Optionen in den Checkboxen ausgewählt sind. Hier wird kein neues Element erzeugt, sondern ein bereits im HTML vorhandenes, verstecktes `<div>` (ID `checkboxErrorField`) manipuliert.

Die Funktion hat daher keinen Parameter, sondern sucht das Feld, setzt darin eine fixe Fehlermeldung und macht das Element sichtbar.

### `clearErrors()`

Diese Funktion wird zu Beginn jeder Validierung in `isValid()` aufgerufen. Sie muss alle visuellen Fehlermeldungen entfernen, bevor neu geprüft wird, damit alte Fehlermeldungen verschwunden sind, bevor neue angelegt werden. Die `<div>`-Elemente unter den `<input>`-Feldern werden gelöscht, das `<div>` für die Checkboxen wird geleert und unsichtbar gestellt.

### `resetCheck()`

Fragt beim Klick auf den Reset-Button mit einem Dialogfenster (OK/Cancel) ab, ob das Formular tatsächlich zurückgesetzt werden soll. Ein solcher Dialog existiert bereits – werfen Sie einen Blick auf die Methoden des `window`-Objekts.

> [!TIP]
> Das DOM und seine Operationen sind zu Beginn oft schwer nachzuvollziehen.
>
> **Nutzen Sie KI zur Erklärung:** Lassen Sie sich DOM-Methoden oder Eigenschaften erklären oder fragen Sie bewusst, wie einzelne Dinge (nicht die gesamte Aufgabe) mit dem DOM gelöst werden können.

## Tipps und Hinweise

- **Bootstrap-Klassen:** Nutzen Sie `is-invalid` für den roten Rahmen und `invalid-feedback` für den Text. Sie müssen kein eigenes CSS schreiben.
- **Barrierefreiheit (A11y):** Durch das Setzen von `aria-invalid="true"` (via JS) und `aria-live="polite"` (ist bereits im HTML beim Checkbox-Fehler gesetzt) stellen wir sicher, dass auch Screenreader die Fehler bemerken (würden).
- **DOM-Traversierung:** Nicht jedes Element im DOM muss gesucht werden. DOM-Knoten enthalten Verweise auf das Elternelement (`parentElement`) oder auf das nächste Geschwistelement (`nextElementSibling`).
- **Postleitzahl**: Die Datei `main.js` enthält die Funktion `isNonNegativeInteger(stringNumber)`, mit der überprüft werden kann, ob ein Wert aus einem Formularfeld eine nicht-negative Ganzzahl (d. h. >= 0) ist. Die Funktion wandelt zunächst mithilfe des `Number`-Konstruktors den String aus dem Formularfeld in eine Zahl um. Mit `Number.isInteger()` wird überprüft, ob das Ergebnis eine Ganzzahl ist. Ergibt dieser Teil `true` wird mittels noch geprüft, ob die Zahl größer oder gleich 0 (also nicht negativ) ist. Dies reicht für eine simple Überprüfung von Postleitzahlen im deutschsprachigen Raum. Dass die Zahl 4 Stellen aufweist, muss gesondert geprüft werden.
- **E-Mail-Adresse:** Strings besitzen die Methode `.includes("Suchtext")`, die `true` oder `false` zurückgibt. Das ist ideal für eine simple E-Mail-Prüfung auf das Zeichen "@".
- **Abfragen mehrerer Elemente eines Kriteriums (für `clearErrors()`):** Beachten Sie die unterschiedlichen Rückgabewerte bei `querySelectorAll()` und den Methoden `getElementsByTagName()` sowie `getElementsByClassName()`. Erstere gibt ihnen eine [`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList), welche statisch ist, also sich in der Länge nicht verändert, auch wenn Sie Elemente daraus löschen. Die anderen beiden Methoden geben eine [`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) zurück, die "live" ist, d. h. gelöschte Elemente verschwinden aus ihr, weshalb etwa beim Durchlaufen mit einer for-Schleife beachtet werden muss, dass sich die Länge reduziert. Mittels [`Array.from()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) können Sie jedoch jederzeit eine statische Kopie einer `HTMLCollection` erstellen.
- Die Chrome DevTools und ähnliche Werkzeuge anderer Browser zeigen generierten JavaScript-Code an.
- MDN als Referenz für [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) und [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) verwenden.
- Bei Fragen oder Problemen zur Aufgabe eröffnen Sie ein Issue in ihrem Repository.