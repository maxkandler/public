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

### Logische Anfrageoptimierung

*Wird gut durch die Folien verdeutlicht, am besten ein Blick ins Skript werfen.*

**Grundideen:**

* Restrukturierung der Anfrage durch mathematische (= relationale Algebra) Regeln
	* Äquivalenzregeln:
		* `JOIN, Vereinigung, Schnitt, Kreuzprodukt` sind kommutativ und assoziativ
		* `Selektionen` sind untereinander vertauschbar
	* `Selektion` und `Projektion` sind vertauschbar (solange keine relevanten Attribute entfernt werden)
	* `Selektionen` können zusammengefasst oder aufgebrochen werden
	* `Projektionen` können in den `Join` verschoben werden
	* `Selektionen` können mit `Vereinigung, Schnitt und Differenz` vertauscht werden
	* `Selektion` und `Join` können vertauscht werden, falls die Selektion nur auf einen der Join-Partner geht
	* `Projektionsoperator` kann mit `Vereinigung` vertauscht werden
	* `Selektion` und `Kreuzprodukt` können zu einem `Join` zusammengefasst werden, wenn Selektionsbedingung der Join-Bedingung entspricht

**Restrukturierungsalgorithmus**

* Aufbrechen der `Selektionen`
* Verschiebe `Selektionen` soweit wie möglich nach unten ([Operatorenbaum](http://de.wikipedia.org/wiki/Datenbankoperator))
* Zusammenfassen von `Selektionen` und `Kreuzprodukten` zu `Joins`
* `Projektionen` soweit wie möglich nach unten verschieben / einfügen
* Zusammenfassen einzelner `Selektionen`

### Kostenmodellbasierte Anfrageoptimierung

Selektivität
: Anteil der qualifizierenden Tupel für die jeweilige Aktion (Selektion, Join)

* Selektivität muss geschätzt werden 
	* **Parametrische Verteilung** (vgl. Normalverteilung, etc.): möglichst passende Funktion und deren Parameter festlegen
	* **Histogramm**: Wertebereich in Intervalle unterteilen und zählen
	* **Stichproben**: `n` Tupel aus einer Relation repräsentativ betrachten

## Implementierung der Joinoperation

### Nested-Loop-Join

* einfachste Variante: Bildung des kartesischen Produkts mit anschließender Selektion
* miserable Performance
* `R` ist äußere Relation, `S` ist innere Relation

		for each Tupel r ∈ R do
			for each Tupel s ∈ S do
				if r.A = s.B then
					result := result ∪ (r×s)


### Nested-Block-Loop-Join

* Tupel werden blockweise geladen 


		for each Block B_R ∈ R do
			load B_R
			for each Block B_S ∈ S do
				load B_S
				for each Tupel r ∈ B_R do
					for each Tupel s ∈ B_S do
						if r.A = s.B then
							result := result ∪ (r×s)

#### Optimierung

**Anzahl Blockzugriffe:**

		B_R + B_S * B_R = Anzahl Blockzugriffe

##### Caching

* Caching Strategien, um die Anzahl der Blockzugriffe niedrig zu halten

1. Seiten der inneren Relation im Cache halten
2. Seiten der inneren Relation im Catche, aber innere Relation jedes zweite Mal rückwärts
3. \|C|-1 Blöcke der äußeren Relation im Cache. Join mit jedem Block der inneren Relation









