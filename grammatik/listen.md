# Listen

Sowohl die Validierung einzelner Objekte aus Listen wie auch Listen als Ganzes lässt sich mit Hilfe von openVALIDATION realisieren. ****Dieser Abschnitt befasst sich mit den Verarbeitungsmöglichkeiten von Listen und stellt die grundlegenden Konzepte und Funktionen auf Listen vor. 



### ERSTE und LETZTE

Wie die Namen der Funktionen `ERSTE` und `LETZTE` vermuten lassen, werden sie dazu verwendet das jeweils erste bzw. letzte Elemente einer Liste zuzugreifen. Mit ihnen lässt sich beispielsweise folgende Regel auf diesem simplen Schema definieren, die jedes Mal fehlschlägt, wenn das erste Element der Liste nicht eins ist:

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

Es sei angemerkt, dass die Funktion `ERSTE` über mehrere Schlüsselwörter genutzt werden kann. Für `ERSTE` gibt es `ERSTEN` und `ERSTER` und ~~~~für `LETZTE` gibt es `LETZTEN` und `LETZTER` als zusätzliche Aliase.

#### 

### MIT \(Bedingungen zur Filterung der Liste\)

Bei der Validierung von Daten ist es häufig notwendig nur jene Elemente einer Liste betrachten, die eine vorgegebene Bedingung erfüllen. Für genau diesen Anwendungsfall existiert die Funktion `MIT`. Sie ermöglicht die  Filterung von Listen nach beliebigen Kriterien sowohl auf Listen mit einfachen Inhalten wie Zahlen als auch mit komplexen Inhalten wie Personen, die wiederum eigene Eigenschaften wie Namen und Alter haben können. Um beispielsweise eine einfache Liste von Zahlen zu filtern, muss man lediglich die zu filternde Listen nach dem Schlüsselwort `AUS` angeben, gefolgt vom Schlüsselwort `MIT` und der jeweiligen Bedingung. Das folgende Szenario stellt eine solche Filterung beispielhaft auf der Liste `Zahlen` dar:

```yaml
Zahlen: [1,2,3,4,5,6,7,8,9,10]
```

Mithilfe des folgenden Ausdrucks lassen sich beispielsweise die Zahlen von 6 bis 10 aus der Liste filtern und in der Variable `MeineListe` verpacken:

```yaml
Alle Elemente AUS Zahlen MIT einem Wert GRÖßER 5 ALS MeineListe
```

Die Filterung von Listen mit komplexen Inhalten gilt als häufiger Anwendungsfall im Rahmen der Validierung. Da sich diese Form der Filterung von der vorangegangen einfacheren Variante unterscheidet, sei zu Orientierung zunächst das folgende Schema gegeben.:

```yaml
Bewerberliste: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19},
    {Name: "Frieda", Alter: 38}]
```

Das Schema besitzt eine Liste mit dem Namen `Bewerberliste`. Jedes Element \(je eins in Zeile 2, 3 und 4\) innerhalb dieser ist einheitlich strukturiert und besitzt jeweils einen eigenen `Namen`und ein eigenes `Alter`. So ließe sich beispielsweise jedes Element als eine Person interpretieren.

Angenommen die Liste der Bewerber soll jetzt nach ihrem Alter gefiltert werden. Nämlich in der Art, dass lediglich Bewerber genommen werden, die jünger sind als 20. Da feststeht, dass jedes Element eine Eigenschaft `Alter` besitzt, kann diese in der Filterung verwendet werden. Einen Ausdruck, der eine solche Filterung durchführt, zeigt dieses Beispiel:

```yaml
Alle Personen AUS der Bewerberliste mit einem Alter KLEINER als 20 ALS Junioren
```

Die Variable `Junioren` enthält nun die beiden Elemente mit dem Namen `Peter` bzw. `Klaus`, da ihre entsprechende Alter kleiner sind als 20. Würde man die resultierende Liste als Schema notieren, würde sie samt ihrem Inhalt wie folgt aussehen:

```yaml
Junioren: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19}]
```

{% hint style="info" %}
Die Reihenfolge der Werte bleibt erhalten.
{% endhint %}

Diese Liste kann dann zur weiteren Validierung genutzt werden.



### ERSTE und LETZTE in Kombination mit Bedingung

Ein weiterer Teil der Funktionalität von `ERSTE` und `LETZTE` ist die Erweiterungsmöglichkeit durch eine Bedingung. Die Bedingung beeinflusst die beiden Funktionen so, dass So gezielter in einer Listen suchen zu können. Die Bedingung wird dabei von dem Schlüsselwort MIT eingeleitet \(siehe dazu auch Abschnitt zur Listen-Funktion MIT\) und sieht innerhalb einer ERSTE-Funktion beispielsweise wie folgt aus:

Jede Person besitzt eine Eigenschaft "Name" und "Alter". Um beispielsweise die Liste aller Alter aus dem Schema zu extrahieren wird lediglich der Name der Liste und der Name der Eigenschaft benötigt, welche man dann mit einem Punkt miteinander verbindet. 

```yaml
Personen.Alter ALS Altersliste
```

```yaml
Die ERSTE Zahl AUS meinen Lieblingszahlen MIT einem Wert GRÖßER als 2 ALS ZahlX

ZahlX MUSS 4 sein 
```

Die Reihenfolge der Werte bleibt dabei erhalten. Das bedeutet zum Beispiel, dass die Variable "Altersliste" die eine Liste mit den drei Werte 21, 54 und 38 in genau dieser Reihenfolge repräsentiert. 

Diese Formulierung bewirkt, dass die Liste solange durchlaufen wird und das erste Objekt ausgegeben wird, das die Bedingung erfüllt.



### Referenz

{% hint style="info" %}
Funktionen müssen immer innerhalb einer Variablen definiert werden.
{% endhint %}

Zur Verarbeitung von Listen stellt openVALIDATION Funktionen zu Verfügung. Mit ihrer Hilfe lassen sich gezielt Elemente extrahieren und Listen filtern, die dann zur weiteren Validierung genutzt werden können. Folgende Tabelle gibt einen Überblick über alle verfügbaren Funktionen:

Die folgende Tabelle gibt einen Überblick über alle verfügbaren Funktionen:

| Funktion | Beschreibung | Aliase |
| :--- | :--- | :--- |
| ERSTE | Gibt die ersten _n_ Elemente der Liste aus. Die Anzahl _n_ ist dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht werden. |  |
| LETZTE | Gibt die letzten _n_ Elemente der Liste aus. Die Anzahl _n_ ist dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht werden. |  |
| MIT | Gibt die Liste aller Elemente zurück, die die Bedingung erfüllen.  |  |

{% hint style="warning" %}
Funktionen müssen immer innerhalb einer Variablen definiert werden.
{% endhint %}



