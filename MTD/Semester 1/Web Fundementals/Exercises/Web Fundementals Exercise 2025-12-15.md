#web_fundementals #javascript 

just javascript basics

```javascript
console.log("Hellow World);

let foo = "tets";
foo = 42; // the meaning of life, the universe and everything
console.log(foo);

const bar = "contsat";

console.log(bar)

const myArray = [1, 2, 3];

console.log(myArray);
console.log(myArray[1]);

myArray[3] = 5; // this works! 
console.log(myArray);

```



idk this is boring

→ more interesting [SVG Clickjacking](https://lyra.horse/blog/2025/12/svg-clickjacking/)

# Übung 9 – JavaScript Teil 2: Objekte im Browser
WEF1UE Web Fundamentals | 13.01.2026 | Wolfgang Hochleitner | Code-along

Zum Kennenlernen des Objektbegriffs und der Ausgabe im Browser in JavaScript soll mit dem Datumsobjekt gearbeitet werden. Es sollen das aktuelle Datum und die Uhrzeit ausgegeben werden. Auf einen Klick soll angezeigt werden, wie lange die aktuelle Seite bereits geöffnet ist, und eine Möglichkeit bestehen, die Seite neu zu laden.

## Die HTML-Datei

Gegeben ist eine einfache HTML-Datei namens `index.html` mit dem Standard-HTML-Grundgerüst. Diese soll zu Beginn um die nötigsten Inhalte erweitert werden. Es genügt eine einfache Struktur mit zwei leeren `<div>`-Elementen mit IDs (als Platzhalter für Datum und Uhrzeit) sowie zwei Buttons. Die Buttons sollen über das `onclick`-Attribut die Berechnung der Verweildauer und das Neuladen der Seite auslösen. Die HTML-Seite muss dabei keinem bestimmten Layout unterliegen und auch nicht per CSS gestaltet sein (kann aber natürlich). Die Abbildung zeigt das Dokument noch ohne JavaScript-Inhalte; beim Klicken auf die Buttons passiert außerdem nichts.

| <img src="stage1.png" alt="Ohne JavaScript steht zwar die Dokumentenstruktur, jedoch fehlen die Inhalte zu Uhrzeit und Datum. Beim Klicken der Links passiert außerdem nichts." width="800"> |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|              **Ohne JavaScript steht zwar die Dokumentenstruktur, jedoch fehlen die Inhalte zur Uhrzeit und zum Datum. Beim Klicken auf die Buttons passiert außerdem nichts.**              |

## Die JavaScript-Datei

Legen Sie nun eine externe JavaScript-Datei (Textdatei, beliebiger Name, Endung `.js`) an und binden Sie diese im `<head>` Ihres Dokuments ein und geben Sie das `defer`-Attribut an. Dies ist notwendig, damit die Abarbeitung von JavaScript erst beginnt, wenn die gesamte HTML-Struktur geladen ist. Definieren Sie in der Datei nun Funktionen für verschiedene Aufgaben:

- Eine Funktion `currentDay()`, die das aktuelle Datum ermittelt und ausgibt. Im Datum sollen folgende Dinge enthalten sein: Wochentag, Tag, Monat (als Kalendermonat, nicht als Zahl), aktuelles Jahr (z. B. Tuesday, January 13, 2026).
- Eine Funktion `currentTime()`, die die aktuelle Uhrzeit ermittelt und ausgibt. Die Uhrzeit soll aus Stunde, Minute, Sekunde bestehen (z. B. 13:09:37).
- Eine Funktion `addLeadingZero(value)`, die mit einem Parameter aufgerufen wird und, wenn nötig, an Zahlen eine führende Null davor hängt, um bei Tag, Stunde, Minute und Sekunde sicherzustellen, dass diese immer mit führender Null angezeigt werden. Der neue Wert mit führender Null wird dann mittels `return` zurückgegeben. Diese Funktion dient als reine Hilfsfunktion und soll immer dann aufgerufen werden, wenn die Datums- und Zeitfunktionen ihren Output schreiben, denn das `Date`-Objekt gibt die Uhrzeit immer ohne führende Null zurück.
- Eine Funktion `timeOnPage()`, die mittels `alert()` anzeigt, wie lange die aktuelle Seite (Stunden, Minuten, Sekunden) bereits im Browser geöffnet ist. Ein Reload setzt diese Angabe automatisch immer wieder auf 00:00:00 zurück.
- Eine Funktion `refresh()`, die die aktuelle Seite neu lädt.

## Verwendung der JavaScript-Funktionen & Hinweise

- Verwenden Sie `document.getElementById()` um die `<div>`-Elemente anhand ihrer ID abzuffragen. Setzen Sie dann über die Eigenschaft `innerHTML` einen HTML-Absatz, der dadurch im `<div>` platziert und auf der Seite angezeigt wird (z.B. `document.getElementById("output").innerHTML = "<p>Hallo Welt!</p>";`).
- Das `Date`-Objekt ist ein vordefiniertes Objekt, das im Browser zur Verwendung bereitsteht. Sie müssen sich aber mit `new` eine Instanz (bzw. vermutlich mehrere) davon anlegen. Informationen zum Objekt und dessen Methoden sowie Verwendungsbeispiele finden Sie bei [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) oder auch bei [SELFHTML](https://wiki.selfhtml.org/wiki/JavaScript/Tutorials/Zeit_%26_Datum).
- Die Methoden, die den Wochentag sowie den aktuellen Monat liefern, geben diese Werte als Zahlen (0 für Sonntag (!), 1 für Montag, ..., 6 für Samstag bzw. 0 für Januar, 1 für Februar, ..., 11 für Dezember) zurück. Erledigen Sie die Zuordnung zu den textuellen Repräsentationen (Sonntag, Montag, Dienstag, ..., Januar, Februar, ...) jeweils über Arrays. Legen Sie sich also Arrays mit den Wochentagen und Monatsnamen an und greifen Sie dann über die Index-Zahl, die Sie von der jeweiligen Methode des Date-Objekts erhalten, zu.
- Rufen Sie die Funktionen zum Ausgeben von Datum und Uhrzeit am Ende Ihrer JavaScript-Datei auf, sodass sie beim Laden der Seite automatisch ausgeführt werden und den Output erzeugen. Die folgende Abbildung verdeutlicht dies.

| <img src="stage2.png" alt="Nach dem Laden der Seite werden automatisch die JavaScript-Funktionen zum Anzeigen von Uhrzeit und Datum aufgerufen. Diese fügen die zwei Zeilen über den Links ein." width="800"> |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| **Beim Laden der Seite werden die JavaScript-Funktionen zum Anzeigen von Uhrzeit und Datum aufgerufen. Diese fügen die zwei Sätze mit Datum und Uhrzeit über den Buttons in den vorgesehenen Elementen ein.** |

- Rufen Sie die Funktionen zum Anzeigen der Verweildauer auf der Seite sowie für das Neuladen jeweils in einem Button mit dem `onclick`-Attribut auf, wie in der folgenden Abbildung gezeigt. Dies gibt die Verweildauer aus.

| <img src="stage3.png" alt="Nach dem Klick auf *Your time on this page* wird die Verweildauer auf der Seite ausgegeben." width="800"> |
|:------------------------------------------------------------------------------------------------------------------------------------:|
|                    **Nach dem Klick auf "Your time on this Page" wird die Verweildauer auf der Seite angezeigt.**                    |

- Um die Zeitdifferenz für die Verweildauer zu berechnen, können Sie die Millisekunden seit 1.1.1970 eines `Date`-Objekts von denen eines zweiten abziehen und daraus ein neues Objekt erzeugen, das nur noch die Differenz enthält. Beachten Sie dabei, dass bei der Ausgabe der Stunde(n) die Zeitzone (derzeit UTC+1 in der Winterzeit) hinzugerechnet wird. Verwenden Sie daher bei der Ausgabe der Verweildauer die verschiedenen UTC-Funktionen des `Date`-Objekts, z.B. `getUTCHours()`, die Zeiten in UTC (also ohne Zeitzonen-Offset) ausgeben.
- Die Eigenschaften des `window`-Objekts und dessen Eigenschaften können beim Reload und bei der Ausgabe mit der Funktion `alert()` hilfreich sein. Beispiele finden sich bei [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window) und [SELFHTML](https://wiki.selfhtml.org/wiki/JavaScript/Window). 

## Tipps und Richtlinien

- Die Chrome DevTools und ähnliche Werkzeuge anderer Browser zeigen generierten JavaScript-Code an.
- Falls Sie bereits mit JavaScript Erfahrung haben und mit dem DOM im Detail vertraut sind, können Sie davon auch mehr nützen (etwa um Event-Listener in JavaScript zu registrieren). Das DOM ist ansonsten der Inhalt von Vorlesung 10 und Übung 10.
- Bei Fragen oder Problemen zur Aufgabe eröffnen Sie ein Issue in Ihrem Repository.