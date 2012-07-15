# Recovery

## Typisierung & Behandlung

**Transaktions-Recovery**

* **Transaktionsfehler** Lokaler Fehler einer noch nicht festgeschriebenen TA
	* Behandlung durch Rücksetzen (lokales UNDO)

**Crash-Recovery**

* **Systemfehler** Fehler mit Hauptspeicherverlust
	* Behandlung durch **Crash Recovery** (Warmstart)
		* Globales UNDO (nicht abgeschlossene TAs, die bereits geschrieben wurden)
		* Globales REDO (abgeschlossene TAs, die noch nicht geschrieben wurden)

**Geräte Recovery**

* **Medienfehler** Fehler mit Hintergrundspeicherverlust
	* Behandlung durch **Geräte Recovery** (Kaltstart)
		* Backup einspielen
		* Globales REDO (alle TAs, die nach dem Backup abgeschlossen wurden)

## Sicherungspunkte

Maßnahme zur Begrenzung des REDO-Aufwands bei Systemfehlern.

**Durchführung von Sicherungspunkten**

* Spezielle Log-Einträge `BEGIN_CHKPT <infos über laufende TAs END_CHKPT`
* LSN des letzten vollständig ausgeführten Sicherungspunktes wird in Restart-Datei geführt

### Direkte Sicherungspunkte

#### TOC: TA-orientierte Sicherungspunkte

* TOC = Force, alle Änderungen werden bei COMMIT ausgeschrieben
* Sicherungspunkt bezieht sich immer auf genau eine TA
* Nur Änderungen der jeweiligen TA werden geschrieben
* Nur UNDO-Recovery nötig

#### TCC: TA-konsistente Sicherungspunkte

* DB wird in TA-konsistenten Zustand gebracht, keine schmutzigen Änderungen.
* Während Sicherung sind keine TAs aktiv
* Sicherungspunkt bezieht sich aus alle TAs
* UNDO & REDO-Recovery sind durch letzten Sicherungspunkt begrenzt
* Ablauf: 
	* Anmelden Sicherungspunkt
	* Warten bis alle Änderungs-TAs abgeschlossen sind
	* Sicherungspunkt erzeugen
	
#### ACC: Aktions-konsistente Sicherungspunkte

* Ähnlich zu TCC
* Blockierung auf Operationsebene, nicht mehr für ganze TAs

### Indirekte Sicherungspunkte

* Änderungen werden nicht vollständig ausgeschrieben
* DB hat keinen konsistenten Zustand, sondern unscharfen *(fuzzy)* Zustand
* **Erzeugen eine indirekten Sicherungspunkts** durch Logging des Status laufender TAs

## Crash Recovery

1. **Analysepahse:** Gewinner & Verlierer TAs bestimmen
2. **REDO-Phase:** Alles wiederholen um Zustand zum Zeitpunkt des Crashs zu erreichen
3. **UNDO-Phase:** Zurücksetzen der Verlierer-TAs in umgekehrter Reihenfolge

> Verlierer-TAs sind solche, die zum Zeitpunkt des Crashs aktiv waren
