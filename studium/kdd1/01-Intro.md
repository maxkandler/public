# Intro zu KDD

## Definition 

> KDD is the nontrivial process of identifying valid, novel, potentially useful, and ultimately understandable patterns in data.

("From Data Mining to Knowledge Discovery in Databases", Usama Fayyad, Gregory Piatetsky-Shapiro, and Padhraic Smyth; 1996 -- [online ref](http://www.aaai.org/ojs/index.php/aimagazine/article/download/1230/1131))

## KDD-Prozessmodell

*nach Fayyad, Piatetsky-Shapiro, Smyth* 


**Input:** Datenbank

1. Fokussieren
2. Vorverarbeitung
3. Transformation
4. Data Mining<br>
Generierung von Mustern
5. Evaluation

**Output:** Wissen

## Data-Mining Techniken

### Unterscheidung supervised – unsupervised

Supervised
: Ergebnis-Merkmale anhand von Vorwissen (i.d.R. Trainingsdaten) schätzen/lernen. *Beispiele:* Klassifikation, Regression, Outlier Detection

Unsupervised
: Unterteilung ohne Vorwissen. *Beispiele:* Clustering, Outlier Detection, Assoziationsregeln


### Clustering

Zerlegung einer Menge von Objekten in Teilmengen (Cluster) ähnlicher Objekte.


### Outlier Detection

Ermittlung von untypischen Daten. (=> Missbrauch, Fehler)


### Klassifikation

Aus Trainingsdaten Regeln "erlenen", mit denen neue Objekte klassifiziert werden können. 

Ergebnismerkmal ist nominal (kategorisch).


### Regression

Ähnlich zu Klassifikation, aber Ergebnis-Merkmal ist metrisch.


### Assoziation

Regeln in einer Datenbank finden. So dass gilt, wenn x,y,z zutreffen, dann auch w mit einer Wahrscheinlichkeit > v.



