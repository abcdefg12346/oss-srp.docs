# Umsetzung

## Datenbank

Die Datenbank ist ein PostgreSQL-System, in welches einmalig manuell die statischen Spiele-Daten importiert werden. Diese Daten werden nun dauerhaft alle 2 Wochen aktualisiert. Hier für hat bietet ein anderes Community-Mitgleid einen PostgreSQL-Dump an, welcher vom System heruntergeladen wird und importiert wird.

In die gleiche Datenbank werden auch die Verlustdaten in eigene Tabellen geschrieben. Hierfür werden für die einzelnen benötigten Bereiche Tabellen erstellt. Diese sind relationell miteinander verknüpft.


## NodeJS

NodeJS ist immernoch relativ experimentell, vor allem verglichen mit den großen, wie Python und PHP. Jedoch vom reinen Umfang her bietet sich NodeJS durchaus an. Es ist derzeit die größte Webumgebung, von der reinen Paketanzahl und -angebot.

Es bietet sich zudem an, NodeJS zu nezten, da es nativ mit JSON arbeiten kann, und damit ideal für den Umgang mit der CREST-API des Entwicklers von EVE Online, 


## Authentification

Die Authentifizierung findet über "CREST", die API des Entwicklers von EVE-Online statt.
Hierzu wird ein Webbrowser aufgerufen, auf dem der User sich mit seinen persönlichen Einloggdaten authentifizieren muss. Das Tool wartet auf Antwort der Seite.
Sobald diese Antwort erfolgt und erfolgreich ist, gilt der User als eingeloggt. 
