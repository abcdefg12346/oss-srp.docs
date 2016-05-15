# Einleitung

Dies ist die Projektdokumentation für das oss-srp-Tool von Alexander Schittler, Niklas Josefsson und Fabian Schiffner im Rahmen des zweiten Ausbildungsjahres an der Berufsbildenden Schule 1 Lüneburg / IT14B. 

## Aufgabenstellung

Ziel ist es eine datenbankgebundene Anwendung zu bauen. Hierbei soll wenn möglich ein Webstack genutzt werden. Desweiteren ist eine Projektdokumentation mit maximal 12 Seiten exklusive Anhang zu erarbeiten. 

## Ziel
_OSS-SRP_ ist ein Service für das Massively-Multiplayer-Online-Spiel "EVE Online". Es dient dazu, das "Ship Replacement Programm" zu vereinfachen. Hierbei werden Verlustdaten analysiert und ausgegeben.

## Anforderungen
---
Unsere Datenquelle ist zum ein statischer Datenxport ("SDE") und zum anderen eine offizielle API des Entwicklers ("CREST"). Die SDE wird mit jeder neuen Version des Spiels aktualisiert, und muss daher mindestens alle 6 Wochen neu importiert werden, womit eine Automatisierung dieses Prozesses naheliegt.

Desweiteren gewünscht ist eine Integration in das bestehende Authentifizierungssystems via CREST, erweiterte Statistiken über User/deren Verluste und benutzerdefinierte Filter.
Auch soll die Lossmail (der Verlust) automatisch auf der Seite "zkillboard.com" hochgeladen werden, da diese von vielen Spielern besucht wird.
---
## Technische Projektumsetzung

### Backend
Diese
Für die Umsetzung haben wir NodeJS und Postgres für das Backend gewählt.
NodeJS bietet sich als Backend an, da es derzeit, dank des modularen Aufbaus durch den Paketmanager _npm_, die größte Plattform für Webanwendungen. NodeJS besticht durch eine gute Perfomance im Vergleich mit anderen Webplattformen und ist sehr skalierbar. Desweiteren ist NodeJS ideal um API's mit JSON-Ausgabe auszuwerten und mit diesen Daten weiter zu arbeiten.

Als Datenbankplattform haben wir PostgreSQL gewählt, da es am nähesten am SQL-Standard ist. Auch ist MySQL nicht von [heruko](https://heroku.com) unterstützt, was ein entscheidener Punkt war, da die Webapp auf heroku gehostet werden soll.

### Frontend

Gewünscht ist ein Frontend auf Basis von AngularJS. Eine tatsächliche Umsetzung des Frontends ist jedoch nicht nötig sondern nur ein Wunschkriterium.
