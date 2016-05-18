# Umsetzung

## Datenbank

Die Datenbank ist ein PostgreSQL-System, in welches einmalig manuell die statischen Spiele-Daten importiert werden sollten. Diese Daten werden  alle 6 Wochen aktualisiert. Hier für hat bietet ein anderes Community-Mitgleid einen PostgreSQL-Dump an, welcher vom System heruntergeladen werden und in die Datenbank importiert werden sollten.

In anbetracht der Skalierbarkeit der Anwendung und zukünftigen Plänen mit der Applikation, war ein solcher statischer Import der Daten nicht nützlich.
Daher haben wir uns entschieden eine eigene "Harvester"-Funktion zu schreiben. Zum einen benötigten wir aus dem PostgreSQL-Dump nur wenige Inhalte, und zum anderen hätten wir eine Art die Verlustdaten "einzusammeln" immer noch benötigt.
Diese Verlustdaten und statischen Daten stehen gemeinsam in einer Datenbank, welche relationell miteinander verknüpft ist.

## NodeJS

NodeJS ist immernoch relativ experimentell, vor allem verglichen mit den großen, wie Python und PHP. Jedoch vom reinen Umfang her bietet sich NodeJS durchaus an. Es ist derzeit die größte Webumgebung, von der reinen Paketanzahl und -angebot.

Es bietet sich zudem an, NodeJS zu nezten, da es nativ mit JSON arbeiten kann, und damit ideal für den Umgang mit der CREST-API des Entwicklers von EVE Online.

Mit der oben beschriebenen _"Harvester"_-Methode sammeln wir nur die von uns benötigten Daten aus den Verlustdaten und speichern Sie in die Datenbank.
Hiermit haben wir nur die gezielten Datensätze die wir für eine weitere Verarbeitung benötigen und vermeiden somit Datenmüll in der Datenbank.
Dies ist, in gekürzter Form, die _"Harvester"_-Methode.
Es wird eine "Killmail" geladen, die entsprechenden, gewünschten Daten geladen und daraufhin in die Datenbank importiert.

```javascript
function Harvester(app) {
	this.addMail = function(mail) {
		const date = moment(mail.killTime, "YYYY-MM-DD HH-mm-ss");
		}
    ['...']
		mail.attackers.forEach(function(attacker) {
				self.cache.corporations.push({date: date, id: attacker.corporation.id, data: attacker.corporation});
        ['...']
			}
		})
	}

	// flush the cache into postgres
	this.cache = {
		alliances: [],
		corporations: [],
		characters: [],
		shipTypes: []
	}
}

module.exports = Harvester;
```

## Authentifizierung

Die Public API _CREST_[^4] ist eine REST API. Dies bedeutet, dass solange die jeweilgen Pfade um auf die API zuzugreifen, dynamisch erzeugt werden, Updates der API nicht unsere App beinflussen.
CREST ist deshalb sinnvoll zu benutzen, da es direkt mit dem Spieleserver kommuniziert und nicht mit der Backend-Datenbank des Entwicklers. Somit hat man einen klareren Zugang zum aktuellen Stand des Spieles. Des weiteren ist es für hohen Traffic und starkes Caching ausgelegt, sodass sie schnell und zuverlässig funktioniert.
Ein großer Teil der Endpoints ist ohne Authentifizierung zu erreichen. Hierzu muss man lediglich den entsprechenden Endpoint aufrufen und kann ohne weiteres mit den JSON-Daten arbeiten.
Jedoch benötigen wir für den Zugriff auf die Verlustdaten eine Authentifizierung, um auf die userspezifischen Daten zugreifen zu können.
Hierzu wird ein _SSO-access-token_ benötigt, welches man erhält, wenn man seine App bei dem Entwickler CCP registriert. Jede Anfrage auf einen Endpoint, welcher Authentifizierung benötigt muss diesen Token beinhalten.

Ebenso wird ein Authentifizierungs-Token für den gewünschten User benötigt, der seine Lossmail importiert. Der User wird beim Aufruf automatisch auf die offizielle Authentifizierungsseite weitergeleitet. Dort muss er seine Einlogg-Daten verwenden und bestätigt, dass er der App tatsächlich diese Daten zur Verfügen stellen möchte.
Der Server antwortet der App, dass der User sich erfolgreich authentifiziert hat und das benötigte Token wird übermittelt. Mit diesem Token kann dann auf die Userdaten zugegriffen und gearbeitet werden.

Dies erreichen wir mit (in Auszügen) folgendem Code:

```javascript
router.get('/crest', function(req, res, next) {
	const endpoint = 'https://login.eveonline.com/oauth/token';
	const params = {
    ['...']
	};
	const auth = {
    ['...']
	};

	request.post(endpoint, {form: params, auth: auth}).then(function(body) {
      ['...']
		const headers = {
			'Authorizatio': 'Bearer ' + tokenData.access_token
		}
		return request(endpoint, {headers: headers});
	}).then(function(body) {
		var characterData = JSON.parse(body);
		return cm.upsert(characterData).then(function() {
			return cm.get(characterData.CharacterID);
		}).then(function(row) {
			res.send({
				token: token,
				data: row
			});
		})
    ['...']
	})
})

module.exports = router;
```
Hier bei wird auf den gewünschten Endpoint mit bestimmten Aufrufargumenten eine Anfrage gestellt. Die API gibt ein JSON-Objekt zurück, welches die Daten zu dem angefragten Charakter erhält.

## Continuous Integration

Die gesamte Umsetzung des Projektes beruht auf "kontinuerlicher Integration". Demnach sind die einzelnen Bereiche und Methoden von einander getrennt und werden jeweils an der Stelle eingebunden an der sie benötigt werden.
Wichtige Grundelemnte beinhalten eine gemeinsame Codebasis, welche über eine Versionsverwaltung läuft, eine häufige Integratino, also ein regelmäßiges Integrrieren von Code in den Hauptbranch des Projektes und kurze Testzyklen, in denen kleine und prägnante Tests abgehalten werden.
So ist man nicht abhänig von Meilensteinen, sondern kann INtegrations-Probleme laufend entdecken und beheben und hat jederzeit eine lauffähige Version des Programmes zu Demo- oder Testzwecken.[^5]

[INSERT TEST HERE]

## Heroku

Heroku ist eine Cload-basierte PaaS (Platform-as-a-Service) welches ausgezeichneten Service für PostgreSQL und NodeJS bietet. Es passt einwandfrei zu unseren Vorraussetzungen und zu unserem _"CI"_-Modell, da es sich gut in einen git-Workflow integrieren lässt. Des weiteren kann man Applikationen sehr leicht dazu konfigurieren automatisch die neuste Version auf Heroku zur Verfügung zu stellen. Hier bei liegt auch die Stärke in der Verbindung mit _"CI"_. 


---
[^4] [CREST-API Dokumentation https://developers.eveonline.com/resource/resources](https://developers.eveonline.com/resource/resources) (abgerufen am 14.05.2015 - 20:00)

[^5] [Kontinuierliche Integration - Wikipedia - https://de.wikipedia.org/wiki/Kontinuierliche_Integration](https://de.wikipedia.org/wiki/Kontinuierliche_Integration) (aufgerufen am 17.05.2016 - 22:25)

