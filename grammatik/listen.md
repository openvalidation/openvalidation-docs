# Listen

Sowohl die Validierung einzelner Objekte aus Listen wie auch Listen als Ganzes lässt sich mit Hilfe von openVALIDATION realisieren. ****Dieser Abschnitt befasst sich mit den Verarbeitungsmöglichkeiten von Listen und stellt die grundlegenden Konzepte und Funktionen auf Listen vor. 



### Erstes und letztes Element einer Liste \(ERSTE- und LETZTE-Funktion\)

Wie die Namen der Funktionen `ERSTE` und `LETZTE` vermuten lassen, werden sie dazu verwendet auf das jeweils erste bzw. letzte Elemente einer Liste zuzugreifen. Mit ihnen lässt sich beispielsweise folgende Regel auf dem unten aufgeführten Schema definieren, die jedes Mal fehlschlägt, wenn das erste Element der Liste nicht `1` ist:

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

### Filterung von Listen mit Bedingung \(MIT-Funktion\)

Bei der Validierung von Daten ist es häufig notwendig nur jene Elemente einer Liste zu betrachten, die eine vorgegebene Bedingung erfüllen. Für genau diesen Anwendungsfall existiert die Funktion `MIT`. Sie ermöglicht die  Filterung von Listen nach beliebigen Kriterien sowohl auf Listen mit einfachen Inhalten wie Zahlen als auch mit komplexen Inhalten wie Personen, die wiederum eigene Eigenschaften wie Namen und Alter haben können. Um beispielsweise eine einfache Liste von Zahlen zu filtern, muss man lediglich die zu filternde Listen nach dem Schlüsselwort `AUS` angeben, gefolgt vom Schlüsselwort `MIT` und der jeweiligen Bedingung. Das folgende Szenario stellt eine solche Filterung beispielhaft auf der Liste `Zahlen` dar:

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

Ein weiterer Teil der Funktionalität von `ERSTE` und `LETZTE` ist die Erweiterungsmöglichkeit durch eine Bedingung. Die Bedingung bezieht sich dabei auf die Elemente der Liste und ermittelt dann das erste bzw. letzte Element, dass die gegebene Bedingung erfüllt. Da sich die beiden Funktionen vom Aufbau nicht unterscheiden, werden die kommenden Beispiele anhand der Funktion `ERSTE` vorgestellt. Sei folgendes Schema mit einer Liste von Zahlen gegeben:

```yaml
Lieblingszahlen: [1,2,4,8,16,32]
```

Das erste Element ließe sich mit diesem Ausdruck aus der Liste filtern:

```yaml
Die ERSTE Zahl AUS meinen Lieblingszahlen ALS Favorit
```

Die Variable `Favorit` würde in diesem Fall die Zahl `1` enthalten. Wollte man nun anstatt `1` die Zahl `4` aus der Liste extrahieren, kann man die obige Funktion durch eine [`MIT` -Funktion](https://app.gitbook.com/@openvalidation/s/documentation/~/drafts/-M-OzcUdmrIsQMlXwpjH/v/de/grammatik/listen#filterung-von-listen-mit-bedingung-mit-funktion) erweitern. Der resultierende Ausdruck sieht dann wie folgt aus:

```yaml
Die ERSTE Zahl AUS meinen Lieblingszahlen MIT einem Wert GRÖßER als 2 ALS Favorit
```

Listen mit komplexen Inhalten können auf diese Weise ebenfalls verarbeitet werden. Das folgende Schema zeigt beispielsweise die Liste `Bewerberliste`, die eine Menge von Personen enthält, welche wiederum ein `Alter` und einen `Namen` haben: 

```yaml
Bewerberliste: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19},
    {Name: "Frieda", Alter: 38}]
```

Um die Person mit dem `Namen` "Peter" aus der Liste zu extrahieren, könnte man sich an den `Altern` der Personen orientieren. Da Peter die erste Person in der Liste ist, die älter ist als 18, ließe er sich mit folgendem Ausdruck extrahieren:

```yaml
Die ERSTE Person AUS der Bewerberliste MIT einem Alter GRÖßER als 18 ALS Peter
```

### 

### Filterung von Listen mit komplexen Inhalten nach Eigenschaft

Eine weitere Form der Filterung auf Listen mit komplexen Inhalten ist die Filterung nach Eigenschaften. Konkret bedeutet das, dass man aus einer Liste solcher komplexen Elemente mit eigenen Eigenschaften eine Liste der Eigenschaften erzeugt. Zur veranschaulichen sei folgendes Schema gegeben:

```yaml
Bewerberliste: [
    {Name: "Peter", Alter: 17},
    {Name: "Klaus", Alter: 19},
    {Name: "Frieda", Alter: 38}]
```

Die Liste `Bewerberliste` ist eine Liste mit drei Personen \(Zeilen 2-4\), wobei jede Person jeweils die Eigenschaften `Name` und `Alter` besitzt. Der folgende Ausdruck dient beispielsweise der Filterung nach dem Alter:

```yaml
Bewerberliste.Alter ALS Altersliste
```

Wie in dem Beispiel zu erkennen ist, erfolgt die Filterung, indem man den Listennamen und die Eigenschaft verbunden mit `.` angibt. Die Reihenfolge der Werte bleibt dabei erhalten. Die Variable `Altersliste` enthält dann die Liste der Alter der Personen aus der `Bewerberliste` und hätte folgenden Inhalt:

```yaml
Altersliste: [17,19,38]
```





### Referenz

{% hint style="info" %}
Funktionen müssen immer innerhalb einer Variablen definiert werden.
{% endhint %}

Zur Verarbeitung von Listen stellt openVALIDATION Funktionen zu Verfügung. Mit ihrer Hilfe lassen sich gezielt Elemente extrahieren und Listen filtern, die dann zur weiteren Validierung genutzt werden können. Folgende Tabelle gibt einen Überblick über alle verfügbaren Funktionen:

Die folgende Tabelle gibt einen Überblick über alle verfügbaren Funktionen:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Funktion</th>
      <th style="text-align:left">Beschreibung</th>
      <th style="text-align:left">Aliase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">ERSTE</td>
      <td style="text-align:left">Gibt die ersten <em>n</em> Elemente der Liste aus. Die Anzahl <em>n </em>ist
        dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht
        werden.</td>
      <td style="text-align:left">ERSTER, ERSTEN</td>
    </tr>
    <tr>
      <td style="text-align:left">LETZTE</td>
      <td style="text-align:left">Gibt die letzten <em>n</em> Elemente der Liste aus. Die Anzahl <em>n </em>ist
        dabei optional und wird als 1 interpretiert, sollte keine Angabe gemacht
        werden.</td>
      <td style="text-align:left">
        <p>LETZTER,</p>
        <p>LETZTEN</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">MIT</td>
      <td style="text-align:left">Gibt die Liste aller Elemente zur&#xFC;ck, die die Bedingung erf&#xFC;llen.</td>
      <td
      style="text-align:left">todo</td>
    </tr>
  </tbody>
</table>{% hint style="warning" %}
Funktionen müssen immer innerhalb einer Variablen definiert werden.
{% endhint %}



