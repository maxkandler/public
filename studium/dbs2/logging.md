# Logging

Aufgabe
: Alle Änderungsoperationen in der DB in einem Protokoll zusammenfassen

## Klassifikation

	 \ Logging
	 |-- physisch
	 | |-- Übergangs-Logging
	 | \-- Zustands-Logging
	 |   |-- Seiten-Logging
	 |   \-- Eintrags-Logging
	 |-- logisch
	 | |-- Übergangs-Logging
	 \-- physiologisch

**Physisches Logging**

* Auf Ebene der physischen Objekte (Seiten, Datensätze, Indexeinträge)
* **Zustands-Logging**
	* Protokollierte Zustände vor und nach Änderung
* **Übergangs-Logging**
	* Protokolliert Zustandsdifferenz vor und nach Änderung

**Logisches Logging**

* Spezialform von Übergangs-Logging, aber unabhängig von der physischen Einheit. Stattdessen Protokollierung der Änderungsoperation auf einer höheren Abstraktionsebene

**Physiologisches Logging**

* Kombination aus Logischem und Physischem Logging: Protokolliert Operationen innerhalb einer Seite

--- 

## Log-Einträge

### Struktur

	( LSN, TA-Id, Page-Id, REDO, UNO, PrevLSN)

LSN (Log Sequence Number)
: Eindeutige Kennung des Log-Eintrages in chronologische Reihenfolge

TA-Id
: ID der ausführenden Transaktion

Page-Id
: ID der betroffenen Seite

REDO
: Informationen, wie Änderungen nachvollzogen werden können

UNDO
: Informationen, wie Änderungen rückgängig gemacht werden können

PrevLSN
: Zeiger auf vorhergehenden Log-Eintrag der jeweiligen TA (für schnellere Navigation)

**Repräsentation meist in Tabellenform:**

| Zeit 	| Aktion 	| Änderung im DB-Puffer 	| Änderung in DB 	| Log-Eintrag im Log-Puffer 	| Zur Log-Datei hinzugefügte Einträge 	|
|------	|--------	|-----------------------	|----------------	|---------------------------	|-------------------------------------	|
| LSN 	| 	| Page-Id, LSN 	| Page-Id, LSN 	| eigentlicher Log-Eintrag 	| LSN-Range der geschriebenen Einträge 	|

### Log-Sätze

`UPDATE`
: nach Modifikation einer Seite wird `pageLSN` auf `LSN` des `UPDATE` Records gesetzt

`COMMIT`
: Record wird mit `TA-Id` geschrieben und auf die Platte geschrieben (Abschluss des `COMMIT`)

`ABORT`
: UNDO wird angestossen

`END`
: Nach Beendigung zusätzlicher Aktionen bei `COMMIT` oder `ABORT`

`CLR (Compensation Log Records)`
: Loggt Aktionen im Zuge von `UNDO`


## Einbringstrategien & Pufferverwaltung

### Einbringstrategien

Wie werden die (veränderten) Daten wieder in die Datenbank geschrieben?

**Direktes Einbringen** (Update-In-Place, Non-Atomic)

* geänderte Seiten überschreiben immer die alte Version
* Ausschreiben der Seite = Einbringen in die Datenbank (kein Zwischenspeicher)
* gängigste Methode

**Indirektes Einbringen** (Atomar)

* geänderte Seite wird in separatem Block geschrieben (Twin-Block-Verfahren, Schattenspeichertechnik)
* atomares Einbringen mehrerer Seiten möglich

### Pufferverwaltung

#### Verdrängungsstrategien

* **Steal** Seiten dürfen jederzeit im Puffer ersetzt und in die DB eingebracht werden
* ⇒ DB kann unbestätigte Änderungen enthalten ⇒ UNDO nötig
* **No-Steal** Seiten dürfen frühestens bei `EOT` in DB aus Puffer entfernt und in DB geschrieben werden
* ⇒ DB enthält nur Änderungen erfolgreicher TAs ⇒ UNDO nicht nötig

#### Ausschreibestrategien (EOT-Behandlung)

* **Force** geänderte Seiten spätestens bei `EOT` in DB schreiben 
* ⇒ REDO nicht nötig
* **No-Force** geänderte Seiten können auch erst nach EOT in die DB geschrieben werden
* ⇒ Änderungen erfolgreicher TAs evtl. nicht in DB ⇒ REDO nötig

> **PRAXIS:** STEAL/NO-FORCE

## WAL & COMMIT

**WAL-Prinzip** (Write-Ahead-Log)

* UNDO-Informationen müssen vor Änderung in der DB im Log stehen
* Nur relevant bei STEAL
* Wichtig bei direktem Einbringen

**COMMIT-Regel** (Force-Log-At-Commit)

* REDO-Informationen muss vor COMMIT im Log stehen
* Vorraussetzung für Geräte-Recover und Crash-Recovery (~ bei No-Force)






