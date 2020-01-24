# IT-Auswahl
Projekt im Rahmen des Moduls "IT-Auswahl und Anpassung"

## Dokumentation Camunda BPM Prozess
Als wir uns innerhalb der Gruppe beraten hatten, überlegten wir den gesamten Wartungsprozess inklusive der Terminvergabe zu wählen und diesen dann im Camunda Modeler zu modellieren. Nach der Abstimmung mit Prof. Dr. Meister ergab sich, dass dieser Prozess für die vorgegebene Zeit zu umfangreich und nicht realisierbar ist. Dieser Prozess sah wie folgt aus:

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/1. Version.jpeg"
alt="erste Version" />

Danach passten wir unseren Prozess an und legten den Fokus auf die Rechnungserstellung im Wartungsprozess. 

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/2. Version.jpeg"
alt="zweite Version" />

Der Prozess war immer noch zu umfangreich und komplex. Ein weiterer Kritikpunkt waren die unpassenden Formulierungen der Tasks und Events. Außerdem fehlte die DMN Task, weshalb wir erneut den Prozess gewechselt hatten. 

Unser präfinaler Prozess beschäftigt sich nun nur mit der Terminvergabe für Wartungen. Der Prozess beginnt mit dem Auslösen einer Wartungsmeldung. Daraufhin wird dieses Formular intern vom Programm "ausgefüllt" und weiter verarbeitet. Der eigentliche Gedanke hierbei war, dass das Programm weiß wann welches Fahrzeug zur Wartung dran ist und schickt dann automatisch die Terminvorschläge los. Die Implementierung dessen wäre jedoch zu aufwendig gewesen. Daher füllt in unserem Prozess ein Mitarbeiter manuell ein Protokoll aus, in welchen die Vertragsart, die Fahrzeugklasse und das Fahrzeugalter angegeben werden muss. Die DMN Task entscheidet dann, ob die Wartung extern oder intern erfolgt. Je nach Ergebnis der DMN Task wird mittels einer exclusive Decision auf den internen oder externen Wartungspfad weitergeleitet. Im Internen Pfad prüft zuerst das System nach möglichen Terminen und schlägt diese dann vor. Der betroffene Mitarbeiter erhält eine Meldung und überprüft ob der Termin wahrgenommen werden kann. Nach seiner Bestätigung wird das Fahrzeug der Werkstatt übergeben und der Prozess (intern) ist abgeschlossen. Im externen Pfad wird die externe Werkstatt mittels einer automatisierten Benachrichtigung per Mail informiert. Dann wird der Termin manuell mit den betroffenen Mitarbeiter abgestimmt. Anschließend wird das Fahrzeug der Werkstatt übergeben und der Prozess (extern) ist abgeschlossen.

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/3. Version.jpeg"
alt="dritte Version" />

An diesem Prozess nahmen wir letzte Änderungen vor und verfeinerten diesen. Eine der Änderungen ist, dass wir uns für ein Message Start Event entschieden haben. Die Wartungsmeldung wird durch eine HTTP POST Request gestartet. 

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/Postman Startnachricht.png"
alt="Postman Startnachricht"/>

Die nächsten zwei Abbildungen beziehen sich auf die Abfrage der DMN Tabelle. Hier wird die DMN Tabelle mit HTTP POST Requests abgefragt. Leider kann eine Automatisierung dieses Prozessschritts durch die REST API von Camunda nicht erreicht werden. Es werden die zwei Möglichkeiten gezeigt, die mit dem Prozess erreicht werden können. Entweder das Fahrzeug kann an die interne Werkstatt oder an die externe Werkstatt weitergegeben werden.

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/Postman DMN Interne Vergabe.png"
alt="Postman Interne Vergabe"/>

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/Postman DMN Externe Vergabe.png"
alt="Postman Externe Vergabe"/>

Dadurch wird der Prozess mit Variablen fortgesetzt. Diese sind in der DMN Tabelle definiert. Noch eine Änderung befindet sich bei der Service Task „Termin vorschlagen“. Hier wird ein http get an localhost nodjs an einen Server gesendet, welcher in einer Demoversion ein Json Object mit einem Datum zurückgibt.

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/KalenderAPI.jpeg"
alt="Kalender API"/>

Die letzte Änderung ist bei der Send Task „Benachrichtigung externer Werkstatt“. Diese sendet eine Email von thbcamunda@gmail an die  hinterlegte Email mit der Wartungsanfrage.
Somit sieht unser finaler Prozess wie folgt aus:

<img src="https://github.com/cooleitmenschen/IT-Auswahl/blob/master/Prozesse/Abbildungen/4. finale Version.jpeg"
alt="finale Version"/>
