# Umsetzung

## Datenbank

Die Datenbank ist ein PostgreSQL-System, in welches einmalig manuell die statischen Spiele-Daten importiert werden. Diese Daten werden nun dauerhaft alle 2 Wochen aktualisiert. Hier für hat bietet ein anderes Community-Mitgleid einen PostgreSQL-Dump an, welcher vom System heruntergeladen wird und importiert wird.

In die gleiche Datenbank werden auch die Verlustdaten in eigene Tabellen geschrieben. Hierfür werden für die einzelnen benötigten Bereiche Tabellen erstellt. Diese sind relationell miteinander verknüpft.


## NodeJS

NodeJS ist immernoch relativ experimentell, vor allem verglichen mit den großen, wie Python und PHP. Jedoch vom reinen Umfang her bietet sich NodeJS durchaus an. Es ist derzeit die größte Webumgebung, von der reinen Paketanzahl und -angebot.

Es bietet sich zudem an, NodeJS zu nezten, da es nativ mit JSON arbeiten kann, und damit ideal für den Umgang mit der CREST-API des Entwicklers von EVE Online.


## Authentifizierung

Die Public API _CREST_ ist eine REST API. Dies bedeutet, dass solange die jeweilgen Pfade um auf die API zuzugreifen, dynamisch erzeugt werden, Updates der API nicht unsere App beinflussen.
CREST ist deshalb sinnvoll zu benutzen, da es direkt mit dem Spieleserver kommuniziert und nicht mit der Backend-Datenbank des Entwicklers. Somit hat man einen klareren Zugang zum aktuellen Stand des Spieles. Des weiteren ist es für hohen Traffic und starkes Caching ausgelegt, sodass sie schnell und zuverlässig funktioniert.
Ein großer Teil der Endpoints ist ohne Authentifizierung zu erreichen. Hierzu muss man lediglich den entsprechenden Endpoint aufrufen und kann ohne weiteres mit den JSON-Daten arbeiten.
Jedoch benötigen wir für den Zugriff auf die Verlustdaten eine Authentifizierung, um auf die userspezifischen Daten zugreifen zu können.
Hierzu wird ein _SSO-access-token_ benötigt, welches man erhält, wenn man seine App bei dem Entwickler CCP registriert. Jede Anfrage auf einen Endpoint, welcher Authentifizierung benötigt muss diesen Token beinhalten.

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
			'Authorization': 'Bearer ' + tokenData.access_token
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

