# Listen

Sowohl die Validierung einzelner Objekte aus Listen wie auch Listen als Ganzes lässt sich mit Hilfe von openVALIDATION realisieren. ****Dieser Abschnitt befasst sich mit den Verarbeitungsmöglichkeiten von Listen und stellt die grundlegenden Konzepte und Funktionen auf Listen vor.

## Von Listen von Objekten zu Listen von Eigenschaften

Für Schemata, die eine oder mehrere Listen von Objekten mit wiederum eigenen Eigenschaften enthalten, bietet openVALIDATION die Möglichkeit, diese zu "verflachen" und aus einer Liste von Objekten mit Eigenschaft X eine Listen von Eigenschaften X zu extrahieren. Sei folgendes Schema mit einer Liste bestehend aus drei Personen gegeben:

```yaml
Personen: [
    {Name: "Peter", Alter: 21},
    {Name: "Klaus", Alter: 54},
    {Name: "Frieda", Alter: 38}]
```

 Jede Person besitzt eine Eigenschaft "Name" und "Alter". Um beispielsweise die Liste aller Alter aus dem Schema zu extrahieren wird lediglich der Name der Liste und der Name der Eigenschaft benötigt, welche man dann mit einem Punkt miteinander verbindet. 

```yaml
Personen.Alter ALS Altersliste
```

Die Reihenfolge der Werte bleibt dabei erhalten. Das bedeutet zum Beispiel, dass die Variable "Altersliste" die eine Liste mit den drei Werte 21, 54 und 38 in genau dieser Reihenfolge repräsentiert. 



### ERSTE und LETZTE

Die Funktionen ERSTE und LETZTE werden verwendet, um auf das jeweils erste bzw. letzte Elemente einer Liste zuzugreifen. Mit ihnen lässt sich beispielsweise folgende Regel auf diesem simplen Schema definieren, die jedes Mal fehlschlägt, wenn das erste Element der Liste nicht eins ist:

```yaml
Lieblingszahlen: [1, 2, 4, 8, 16]
```

```yaml
Die ERSTE Zahl AUS meinen Lieblingszahlen ALS erste Lieblingszahl

Meine erste Lieblingszahl MUSS eine 1 sein
```

Des weiteren ist es möglich, sich mehrere Elemente einer Liste auf einmal zu holen, indem man der Funktion eine Anzahl mitteilt. Zum Beispiel enthält die folgende Variable die Liste der ersten drei Elemente der Liste:

```yaml
Die ERSTEN 3 Zahlen AUS meinen Lieblingszahlen ALS Top 3
```

{% hint style="info" %}
Sollte die Anzahl der gewünschten Elemente die Länge der Liste überschreiten, wird die gesamte Liste zurückgegeben.
{% endhint %}

Es sei angemerkt, dass die Funktion ERSTE über mehrere Schlüsselwörter genutzt werden kann. Für ERSTE gibt es ERSTEN und ERSTER und für LETZTE gibt es LETZTEN und LETZTER als zusätzliche Aliase.

#### 

### 

### MIT \(Bedingungen zur Filterung der Liste\)

Mit der MIT-Funktion ist es möglich Listen nach bestimmten Kriterien zu filtern. Dabei hat man die Möglichkeit, sowohl auf Listen von einfachen Werten, wie Zahlen und Wörtern, als auch Listen von komplexen Objekten mit eigenen Eigenschaften zu arbeiten. Sei für den einfachen Fall zunächst folgendes Schema gegeben:

```yaml
Zahlen: [1,2,3,4,5,6,7,8,9,10]
```

Mithilfe der folgenden Ausdrucks lassen sich beispielsweise die Zahlen von 6 bis 10 aus der Liste filtern und in einer Variable verpacken:

```yaml
Alle Elemente AUS Zahlen MIT einem Wert GRÖßER 5 ALS MeineListe
```

Die Filterung von Listen mit komplexen Inhalten gilt als häufiger Anwendungsfall im Rahmen der Validierung. Da sich diese Form der Filterung von der vorangegangen einfacheren Variante unterscheidet, sei zu Orientierung zunächst das folgende Schema gegeben.:

```yaml
Personen: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19},
    {Name: "Frieda", Alter: 38}]
```

Das Schema besitzt eine Liste mit dem Namen Bewerber. Jedes Element \(je eins in Zeile 2, 3 und 4\) innerhalb dieser ist einheitlich strukturiert und besitzt jeweils einen eigenen `Namen`und ein eigenes `Alter`.  So ließe sich beispielsweise jedes Element als eine Person interpretieren.

Angenommen die Liste der Bewerber soll jetzt nach ihrem Alter gefiltert werden. Nämlich in der Art, dass lediglich Bewerber genommen werden, die jünger sind als 20. Eine Regel, die eine solche Filterung durchführt, zeigt dieses Beispiel:

```yaml
Die ERSTE Person AUS Bewerber mit einem Alter KLEINER als 20 ALS Junior
```

```yaml
Alle Elemente AUS Personen mit einem Alter KLEINER als 20 ALS Auszubildende
```

Die Variable `Junioren` enthält nun die beiden Elemente mit dem Namen `Peter` bzw. `Klaus`, da ihre entsprechende Alter kleiner sind als 20. Würde man die resultierende Liste als Schema notieren, würde sie samt ihrem Inhalt wie folgt aussehen:

```yaml
Junioren: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19}]
```

Diese Liste kann dann zur weiteren Validierung genutzt werden.

### ERSTE und LETZTE in Kombination mit Bedingung

Ein weiterer Teil der Funktionalität von ERSTE und LETZTE ist die Möglichkeit zusätzlich eine Bedingung hinzuzufügen und so gezielter in einer Listen suchen zu können. Die Bedingung wird dabei von dem Schlüsselwort MIT eingeleitet \(siehe dazu auch Abschnitt zur Listen-Funktion MIT\) und sieht innerhalb einer ERSTE-Funktion beispielsweise wie folgt aus:

```yaml
Die ERSTE Zahl AUS meinen Lieblingszahlen MIT einem Wert GRÖßER als 2 ALS ZahlX

ZahlX MUSS 4 sein 
```

Diese Formulierung bewirkt, dass die Liste solange durchlaufen wird und das erste Objekt ausgegeben wird, das die Bedingung erfüllt.

{% hint style="info" %}
Sollte kein Element die Bedingung erfüllen, gelten automatisch alle Bedingungen, die die jeweilige Variable nutzen als nicht erfüllt.
{% endhint %}

## Referenz

Zur Verarbeitung von Listen stellt openVALIDATION Funktionen zu Verfügung. Mit ihrer Hilfe lassen sich gezielt Elemente extrahieren und Listen filtern, die dann zur weiteren Validierung genutzt werden können. Folgende Tabelle gibt einen Überblick über alle verfügbaren Funktionen:

| Funktion | Beschreibung | Aliase |
| :--- | :--- | :--- |
| ERSTE | Gibt die ersten _n_ Elemente der Liste aus. Die Anzahl _n_ ist dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht werden. |  |
| LETZTE | Gibt die letzten _n_ Elemente der Liste aus. Die Anzahl _n_ ist dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht werden. |  |
| MIT | Gibt die Liste aller Elemente zurück, die die Bedingung erfüllen.  |  |

{% hint style="warning" %}
Funktionen müssen immer innerhalb einer Variablen definiert werden.
{% endhint %}



