# Anfragebearbeitung


## Anfrageoptimierung


### Grundlage

**Kanonischer Auswertungsplan** orientiert sich an der Logik, die sich aus einer einfachen SQL-Anfrage ergibt.

**BSP:**

	SELECT A1, A2
	FROM R1, R2
	WHERE B1 = B2

1. Bilde kartesisches Produkt der Relationen R1 & R2
2. Führe Selektion mit B1 = B2 durch
3. Ergebnis-Tupel auf A1 und A2 projizieren

### Modelle zur Anfragenoptimierung

Logische Anfrageoptimierung
: ändert den Auswertungsplan (Reihenfolge der Operatoren ändern)

Physische Anfrageoptimierung
: Auswahl geeigneter Auswertungsstrategien und Benutzen von Indices. 

Regelbasierte Anfrageoptimierung
: Fixes Set an Regeln sind die einzige Grundlage zur Optimierung von Anfragen

Kostenbasierte Anfrageoptimierung
: Erweitert die *Regelbasierte Anfrageoptimierung* um die Einschätzung der Performanz solche Regeln und entscheidet sich für die performanteste Variante

**Vorgehen von kostenbasierten Anfrageoptimierern**

* initialer Plan (meist Standardauswertung)
* Schätze Kosten für Auswertung
* Modifiziere Plan gemäß Heuristiken
* Wiederhole 2 & 3 bis zu ein vordefiniertes Kriterium erreicht wurde
* Gib besten Plan (= geringste Kosten) zurück




