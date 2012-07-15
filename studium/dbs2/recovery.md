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

d
