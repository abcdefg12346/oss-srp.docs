# Analyse

## Soll-Kriterien
Wenn man einen Vergleich zwischen den gesetzten Anforderungen und dem Erreichten zieht, kommt man zu dem Ergebniss, dass wir unsere Soll-Ziele erfüllt haben.
Wir haben ein funktionierendes Backend geschrieben, welches nach einer Authentifizierung mit dem offiziellen Service, Verlustdaten importieren kann und diese nach grundlegenden Kriterien automatisch filtert.

## Kann-Kriterien
Das gesetzte Ziel, die statischen Daten des Entwicklers CCP von EVE Online automatisch in regelmäßigen Abständen in unsere Datenbank zu importieren, haben wir ebenfalls erreicht. So wird nun mit einem Crontab alle 6 Wochen automatisch vom System als PostgreSQL-Dump heruntergeladen und in das System importiert. 
