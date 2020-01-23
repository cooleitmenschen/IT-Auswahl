# IT-Auswahl
Projekt im Rahmen des Moduls "IT-Auswahl und Anpassung"

## Dokumentation Camunda BPM Prozess
Als wir uns innerhalb der Gruppe beraten hatten, überlegten wir den gesamten Wartungsprozess inklusive der Terminvergabe zu wählen und diesen dann im Camunda Modeler zu modellieren. Nach der Abstimmung mit Prof. Dr. Meister ergab sich, dass dieser Prozess für die vorgegebene Zeit zu umfangreich und nicht realisierbar ist. Dieser Prozess sah wie folgt aus:

siehe Abb. 1 (alle Abbildungen sind in der Abbildungsverzeichnis.pdf)

Danach passten wir unseren Prozess an und legten den Fokus auf die Rechnungserstellung im Wartungsprozess. 

siehe Abb. 2

Der Prozess war immer noch zu umfangreich und komplex. Ein weiterer Kritikpunkt waren die unpassenden Formulierungen der Tasks und Events. Außerdem fehlte die DMN Task, weshalb wir erneut den Prozess gewechselt hatten. 

Unser präfinaler Prozess beschäftigt sich nun nur mit der Terminvergabe für Wartungen. Der Prozess beginnt mit dem Auslösen einer Wartungsmeldung. Daraufhin wird dieses Formular intern vom Programm "ausgefüllt" und weiter verarbeitet. Der eigentliche Gedanke hierbei war, dass das Programm weiß wann welches Fahrzeug zur Wartung dran ist und schickt dann automatisch die Terminvorschläge los. Die Implementierung dessen wäre jedoch zu aufwendig gewesen. Daher füllt in unserem Prozess ein Mitarbeiter manuell ein Protokoll aus, in welchen die Vertragsart, die Fahrzeugklasse und das Fahrzeugalter angegeben werden muss. Die DMN Task entscheidet dann, ob die Wartung extern oder intern erfolgt. Je nach Ergebnis der DMN Task wird mittels einer exclusive Decision auf den internen oder externen Wartungspfad weitergeleitet. Im Internen Pfad prüft zuerst das System nach möglichen Terminen und schlägt diese dann vor. Der betroffene Mitarbeiter erhält eine Meldung und überprüft ob der Termin wahrgenommen werden kann. Nach seiner Bestätigung wird das Fahrzeug der Werkstatt übergeben und der Prozess (intern) ist abgeschlossen. Im externen Pfad wird die externe Werkstatt mittels einer automatisierten Benachrichtigung per Mail informiert. Dann wird der Termin manuell mit den betroffenen Mitarbeiter abgestimmt. Anschließend wird das Fahrzeug der Werkstatt übergeben und der Prozess (extern) ist abgeschlossen.

siehe Abb. 3

An diesem Prozess nahmen wir letzte Änderungen vor und verfeinerten diesen. Eine der Änderungen ist, dass wir uns für ein Message Start Event entschieden haben. Somit wird eine HTTP Request gestartet, welche den Prozess Wartungsmeldung auslöst. Die nächste Änderung bezieht sich auf die Implementierung einer Postman Meldung welche an den Server gesendet wird. 

siehe Abb. 4

Dadurch wird der Prozess mit Variablen fortgesetzt. Diese sind in der DMN Tabelle definiert. Noch eine Änderung befindet sich bei der Service Task „Termin vorschlagen“. Hier wird ein http get an localhost nodjs an einen Server gesendet, welcher in einer Demoversion ein Json Object mit einem Datum zurückgibt.

siehe Abb. 5

Die letzte Änderung ist bei der Send Task „Benachrichtigung externer Werkstatt“. Diese sendet eine Email von thbcamunda@gmail an die  hinterlegte Email mit der Wartungsanfrage.
Somit sieht unser finaler Prozess wie folgt aus:

siehe Abb. 6
