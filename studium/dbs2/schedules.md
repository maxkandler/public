# Schedules 

Schedule
:	Folge von Aktionen einer Menge von Transaktionen

## Serialisierbare Schedules 

Es gilt,

* ein Schedule ist serialisierbar, wenn der Serialisierungsgraph zyklenfrei ist
* es kann mehrere serielle Schedules für einen Serialisierungsgraph geben
* nur serialisierbare Schedules sind "erlaubt"

### Serialisierungsgraph

Knoten = Transaktionen
Kanten = Abhängigkeiten

## Anomalien

### Lost Update 

$$ S = (r_i(x), w_j(x), w_i(x)) $$

### Dirty Read

$$ S = (w_i(x), r_j(x), w_i(x)) $$

### Non-Repeatable Read

$$ S = (r_i(x), w_j(x), r_i(x)) $$

## Rücksetzbarkeit

Eine Transaktion `T_i` darf erst dann den `COMMIT` durchführen, wenn alle Transaktionen `T_j`, von denen sie Daten gelesen hat, beendet sind.

## Legalität

Ein Schedule gilt als legal, wenn

* vor jedem Zugriff auf ein Objekt eine geeignete Sperre gesetzt wird
* keine Transaktion eine Sperre anfordert, die sie schon besitzt
* spätestens bei Transaktionsende alle Sperren zurückgegeben werden
* Sperren respektiert werden

## Sperrprotokolle

## 2PL – 2-Phasen-Sperrprotokol

* Jede TA durchläuft 2 Phasen: 
	* Wachstumsphase (LOCK)
	* Schrumpfungsphase (UNLOCK)
* diese Phasen dürfen sich nicht überlappen

**Striktes 2PL**

* alle Sperren werden bis zum `COMMIT` gehalten
* `COMMIT` wird atomar durchgeführt

### RX Sperrprotokoll

* Erlaubt Parallelität unter Lesern (mehrere R-Sperren parallel möglich)

### RUX Sperrprotokoll

* (Bei RX) Deadlockgefahr, wenn eine R-Sperre in eine X-Sperre umgewandelt wird (*Sperrkonversion*)
* Zusätzliche U-Sperre, die später in  X oder R umgewandelt werden kann
* U-Sperre erlaubt keine zusätzlichen R-Sperren
* **kein Verhungern möglich**

| 	| R 	| U	| X	|
-	| ---	| ---	| --	|
 R	| +	| -	| -	|
 U	| +	| -	| -	|
 X	| -	| -	| -	|
[*→ vorhandene Sperre ↓ angeforderte Sperre*]

### RAX Sperrprotokoll

* äquivalent zu RUX, jetzt aber bei gesetzter A-Sperre, weitere R-Sperren möglich
* bei Konvertierung A → X **verhungern möglich**
* Tradeoff zwischen Parallelität und Verhungern

| 	| R 	| A	| X	|
|-	|---	|--	|--	|
R 	| +	| +	| -	|
A 	| +	| -	| -	|
X 	| -	| -	| -	|

### RIX Sperrprotokoll 

**Hierarchisches Sperrprotokoll**

* verschiedene Sperrren auf den verschiedenen Granularitätsstufen (Relation – Tupel)
* sparrt Überprüfungsaufwand, wenn neue Sperren angefordert werden

**IR-Sperre**
: auf einer feineren Ebene ist eine R-Sperre *(Intention Read)*

**IX-Sperre**
: auf einer feineren Ebene ist eine X-Sperre *(Intention Exclusive)*

**RIX-Sperre**
: volle R-Sperre + feinere X-Sperre

| 	| R	| X	| IR	| IX	| RIX	|
|--	|--	|---	|---	|----	|
|R	| +	| -	| +	| -	| -	|
|X 	| -	| -	| -	| -	| -	|
|IR 	| +	| -	| +	| ×	| ×	|
|IX	| -	| -	| ×	| ×	| -	|
|RIX	| -	| -	| ×	| -	| -	|
[× zusätzliche Überprüfung auf feinerer Eben notwendig]

