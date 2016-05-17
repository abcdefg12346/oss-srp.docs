# Analyse

## Soll-Kriterien
Wenn man einen Vergleich zwischen den gesetzten Anforderungen und dem Erreichten zieht, kommt man zu dem Ergebniss, dass wir unsere Soll-Ziele erfüllt haben.
Wir haben ein funktionierendes Backend geschrieben, welches nach einer Authentifizierung mit dem offiziellen Service, Verlustdaten importieren kann und diese nach grundlegenden Kriterien automatisch filtert.

## Kann-Kriterien

Anstatt wie vorher das gesetzte Ziel eines statischen Imports von Daten des Entwicklers zu nutzen, haben wir uns dazu entschieden einen _"Harvester"_ zu bauen, welcher nur die tatsächlich benötigten Daten importiert.
Dies erlaubt uns weit mehr Flexibität und Skaliberbarkeit für weitere Ausbaumöglichkeiten derAnwendung. Auch entsteht so eine wesentlich kleinere Datenbank, da wir keinen Datenmüll ansammeln.

## Wunsch-Kriterien
Als Wunsch-Kriterium haben wir uns gesetzt, ein Frontend für das System zu entwickeln. Dieses soll dazu dienen, die Verlustdaten inspizieren zu können und manuell die Daten zu evaluieren. Dieses Kriterium haben wir aus zeitlichen Gründen nicht mehr integrieren können.

## Zeit

| Phase | Soll | Ist | Differenz |
|---|---|---|---|
| Kickoff | 1h | 1h | 0h |
| Planung | 2h | 2h | 0h |
| Umsetzung | 18h | 18h | 0h |
| Dokumentation | 4h | 6h | +2h |
| Gesamt | 25h | 27h | +2h |

Somit lässt sich feststellen, dass wir das Projekt zwar im vorgegebenen Zeitraum der Projektdauer geschafft haben, jedoch das von uns gesetzte Ziel leicht überschritten haben. Dies ist einer Fehlkalkulation der Dauer für die Projektdokumentation zu schulden.

## Lessons learned

Dank des Projektes sind uns einige Dinge in Erfahrung getreten. Zum einen war die Wahl der technischen Grundlage für das Projekt ideal gewählt, jedoch war es nicht an die Fähigkeiten der Gruppenmitglieder angepasst.
So kam es, dass man sich stärker als sonst erst in die Materie einarbeiten musste, da nur ein Gruppenmitglied große Erfahrungen mit NodeJS und PostgreSQL hatte. Diesen Missstand haben wir durch eine kluge Arbeitsaufteilung gelöst. Deshalb konnten wir auch unseren Zeitplan noch im Rahmen einhalten.
Desweiteren haben wir den Aufwand für eine Projektdokumentation unterschätzt. So haben wir für diesen Teil mehr Zeit benötigt als wir ursprünglich kalkuliert haben. Hier hätten wir uns einen größeren Zeitpuffer erarbeiten können, wenn wir den Aufwand besser eingeschätzt hätten.

