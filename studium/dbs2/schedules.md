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

### 2PL – 2-Phasen-Sperrprotokol

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
| :---	| :---	| :---	| :--	|
 R	| +	| -	| -	|
 U	| +	| -	| -	|
 X	| -	| -	| -	|
[*→ vorhandene Sperre ↓ angeforderte Sperre*]

### RAX Sperrprotokoll

* äquivalent zu RUX, jetzt aber bei gesetzter A-Sperre, weitere R-Sperren möglich
* bei Konvertierung A → X **verhungern möglich**
* Tradeoff zwischen Parallelität und Verhungern

| 	| R 	| A	| X	|
| :---	| :---	| :--	| :--	|
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
| :--	| :--	| :---	| :---	| :----	| :--- 	|
| R	| +	| -	| +	| -	| -	|
| X 	| -	| -	| -	| -	| -	|
| IR 	| +	| -	| +	| ×	| ×	|
| IX	| -	| -	| ×	| ×	| -	|
| RIX	| -	| -	| ×	| -	| -	|
[× zusätzliche Überprüfung auf feinerer Eben notwendig]

### RAC Sperrprotokoll (Mehrversionen-Sperre)

**Prinzip:**

* **A-Sperre** um an lokalen Kopien Änderungen vorzunehmen
* **C-Sperre** zeigt an, dass es zwei Versionen gibt
* Bei `COMMIT` A → C
* C-Sperre wird erst freigegeben, wenn letzter alter Leser fertig ist

**Eigenschaften:**

* Leseanforderungen nie blockiert
* Schreiber müssen bei C-SPerre auf alle Leser der alten Version warten
* Hoher Aufwand für Datensicherheit (Integrität) und Serialisierung

| 	| R	| A	| C	|
| :--	| :--	| :--	| :--	|
| R	| +	| +	| +	|
| A	| +	| -	| -	|
| C	| +	| -	| -	|


### Konsistenzstufen

*nach J. Gray*

* Definition über Dauer der Sperren
	* lange = Sperre wird bis EOT gehalten
	* kurz = Sperre wird nicht bis EOT gehalten

| 	| Schreibsperre	| Lesesperre	| Dirty Read 	| Lost Update	| Non-Repeatable Read 	|
| :--	| :--------------	| :-----------	| :----------- 	| :-----------	| :----------- 	|
| Konsistenzstufe 0	| kurz	| keine	| ja	| ja 	| ja 	|
| Konsistenzstufe 1	| lang	| keine	| ja	| nein	| ja	|
| Konsistenzstufe 2	| lang	| kurz	| nein	| ja	| ja	|
| Konsistenzstufe 3	| lang	| lang	| nein 	| nein	| nein	|




**Konsistenzstufe 3**
: entspricht strengem 2PL, Serialisierbarkeit gewährleistet; kein Non-Repeatab

Zusätzlich **Cursor Stability**
: Modifikation von Stufe 2; Lesesperren bleiben bis Cursor zum nächsten Objekt übergeht


## Deadlocks

Strategien zur Vermeidung und Auflösung von "Verklemmungen".

### Preclaiming

* Alle Sperrenanforderungen werden zu Beginn einer TA gestellt

### Ordnung

* Alle Datenbank-Objekte haben eine totale Ordnung
* Sperren werden nur in aufsteigender Reihenfolge vergeben

### Zeistempel

* Jede Transaktion bekommt einen Zeitstempel (i.d.R. die erste Datenbankoperation)

#### wound – wait 

`T_i` fordert Sperre an.

* Jüngere TA hat Sperre auf X 
	* ⇒ `T_i` läuft weiter, jüngere wird zurückgesetzt (**wound**)
* Ältere TA hat Sperre auf X
	* ⇒ `T_i` wartet auf Freigabe der Sperre (**wait**)

#### wait – die

`T_i` fordert Sperre an.

* Jüngere TA hat Sperre auf X
	* ⇒ `T_i` wartet auf Freigabe der Sperre (**wait**)
* Ältere TA hat Sperre auf X
	* ⇒ Ältere TA darf weiterlaufen, `T_i` wird zurückgesetzt (**die**)

## Synchronisation ohne Sperren

* Objekte haben Zeitstempel
	* `readTS(o)` – Zeitstempel der jüngsten TA, die `o` gelesen hat
	* `writeTS(o)` – Zeitstempel der jüngsten TA, die `o` geschrieben hat
* Prüfung bei Lesezugriff von TA `T` auf `o`
	* jüngere TA hat `o` geschrieben ⇒ `T` zurücksetzen
	* sonst: `T` darf `o` lesen, Lesemarke aktualisieren `readTS(o) = max(TS(T), readTS(o))`
* Prüfung bei Schreibzugriff von TA `T` auf `o`
	* jüngere TA hat `o` gelesen oder geschrieben ⇒ `T` zurücksetzen
	* sonst: `T` darf `o` schreiben, Schreibmarke wird aktualisiert `writeTS(o) = TS(T)`

## Optimistische Synchronisation

* Konflikte werden erst bei `COMMIT` festgestellt und dann TAs zurückgesetzt
* `RS(T)` – von `T` gelesene Objekte *(Read Set)*
* `WR(T)` – von `T` geschriebene Objekte *(Write Set)* enthält auf `RS(T)`

### BOCC (Backward-Oriented Optimistic Concurrency Control)

* Validierung nur gegenüber TAs, die während der Ausführung von `T` beendet wurden
* Prüfung bei EOT von `T`: `RS(T)` wird mit `WS(T_j)` verglichen. Konflikt falls Schnittmenge ⇒ `T` zurücksetzen
* Problem: Rücksetzen kann ohne tatsächlichen Konflikt passieren
* Lösung: **BOCC+** Objekte bekommen Versionsnummern

### FOCC (Forward-Oriented Optimistic Concurrency Control)

* Validierung nur gegenüber noch laufenden TAs
* Prüfung bei EOT von `T`: `WS(T)` wird mit `RS(T_j)` verglichen. Konflikt falls Schnittmenge ⇒ eine der betroffenen TAs zurücksetzen
* Vorteil: nur echte Konflikte
