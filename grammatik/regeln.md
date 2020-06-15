# Regeln

Eine Regel ist das wichtigste Konstrukt in der Grammatik von openVALIDATION. Sie bestehen aus einer **Bedingung** und einer **Aktion**. Bei einer **Validierungsregel** ist die Aktion immer eine Fehlermeldung. Die einfachste Möglichkeit so eine Regel auszudrücken ist ein **WENN** / **DANN** Konstrukt. Wenn die Bedingung  wahr ist, dann wird eine Fehlermeldung erzeugt.

```coffeescript
WENN das Alter des Bewerbers KLEINER 18 ist
DANN Sie müssen mindestens 18 Jahre alt sein
```

_Wir schreiben Schlüsselwörter hier in Großbuchstaben, um sie klar erkenntlich zu machen, aber tatsächlich ignoriert openVALIDATION Groß- und Kleinschreibung in Schlüsselwörtern und Regeln können einfach so heruntergeschrieben werden, wie man es auch in gewöhnlichem Text tun würde._

| SCHLÜSSELWORT | TYP | Beschreibung |
| :--- | :--- | :--- |
| WENN | Regel | Markiert den Anfang einer Regel und der darauffolgenden Bedingung |
| DANN | Regel | Markiert den Beginn einer Fehlermeldung |
| KLEINER | Vergleichsoperator | Ein Vergleichsoperator '&lt;' für numerische Operanden |



## Bedingung

Schauen wir uns die Bedingung nochmal etwas genauer an:

```coffeescript
das Alter des Bewerbers KLEINER 18 ist
```

Diese Bedingung besteht aus einer **Vergleichsoperation**. Eine Vergleichsoperation hat immer einen **linken** und einen **rechten Operanden** und den entsprechenden **Vergleichsoperator**.   


## Vergleichsoperatoren

Den Vergleichsoperator `KLEINER` haben wir bereits kennengelernt.  Die Vergleichsoperatoren haben verschiedene Aliases. Damit soll es möglich sein die Regeln grammatikalisch möglichst korrekt auszudrücken. Folgende Aliases gibt es bereits für den Vergleichsoperator `KLEINER`. 

* KÜRZER
* WENIGER
* NIEDRIGER
* GERINGER

Wir könnten einen dieser Aliase verwenden, um beispielsweise folgenden Ausdruck zu definieren:

```coffeescript
der Preis ist GERINGER als 3 Euro
```

Mit der Zeit soll diese Liste weiter vervollständigt werden außerdem wird es in der Zukunft möglich sein benutzerdefinierte Aliases hinzuzufügen.  


## Operanden

Die Erkennung der Operanden ist nämlich das magische an der Grammatik. Wenn wir den Inhalt auf der linken und auf der rechten Seite des Vergleichsoperators genauer betrachten, dann stellen wir fest, dass es aus vielen einzelnen Wörtern besteht. Ein Mensch versteht sofort, dass das Attribut **Alter** und die Zahl **18** die relevanten Werte sind. Eine Maschine dagegen sieht lediglich einen Haufen aneinander gereihter Wörter. 

`das Alter des Bewerbers` und `18 ist`

Betrachten wir zunächst den rechten Operanden: 

`18 ist`

Die Auflösung dieses Operanden hängt vom Datentyp des gegenüberliegenden linken Operanden ab. Sollte der linke Operand ein numerischer Wert sein, so wird die Zahl 18 automatisch aus dem rechten Operanden extrahiert.   


## Semantical Sugar

Das übrig gebliebene Wort `ist` ist für die maschinelle Verarbeitung irrelevant und wird von openVALIDATION einfach ignoriert.  Solchen Inhalt bezeichnen wir als **Semantical oder Grammatical Sugar**. Er ermöglicht es, Regeln ausgeschmückt und grammatikalisch korrekt auszudrücken. Dadurch wird das gesamte Regelwerk für den Menschen lesbarer und Regeln intuitiver zu formulieren.

Es kommt also darauf an, wie der Operand auf der linken Seiten erkannt wird:

`das Alter des Bewerbers`

Das relevante Attribut ist das Alter. Noch kennt openVALIDATION dieses Attribut nicht. Das müssen wir bekanntmachen, indem wir zusätzlich zu einer Regel auch ein **Schema** angeben, welches das entsprechende Datenmodell mit dem jeweiligen Attribut definiert. 

## Schema

Das **Schema ist essentiell** für die Auflösung der Operanden, da Attribute in den Operanden unserer Regeln nur erkannt werden, wenn diese auch im Schema vorhanden sind. Schauen wir uns das Prinzip weiter am Operanden `das Alter des Bewerbers`

Betrachten wir folgendes, stark simplifiziertes Schema, welches lediglich Informationen über das Attribut Alter enthält:

{% tabs %}
{% tab title="YAML" %}
```yaml
Alter: 0
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    Alter: 0
}
```
{% endtab %}
{% endtabs %}

Das Schema kann als JSON Schema oder in einer vereinfachten Form als JSON Object übergeben werden. Da das Schema nun ein Attribut namens `Alter` enthält, wird der Operand als solcher erkannt:

das `Alter` des Bewerbers

Der restliche Inhalt `das ... des Bewerbers` wird dagegen als Semantical Sugar deklariert und von openVALIDATION einfach ignoriert. Der für die Maschine relevante Regelausdruck sieht nach dem Auflösungsprozess letztendlich so aus:

```coffeescript
WENN Alter KLEINER 18
DANN Sie müssen mindestens 18 Jahre alt sein
```

und hier ist der entsprechende formale Ausdruck:

```javascript
if ( Alter < 18 ) 
   throw Error("Sie müssen mindestens 18 Jahre alt sein")
```



## Fehlermeldung

Der gesamte Text hinter dem Schlüsselwort **DANN** wird als Fehlermeldung erkannt:

```coffeescript
Sie müssen mindestens 18 Jahre alt sein
```

Fehlermeldungen enthalten oft eigene Fehlercodes zur eindeutigen Identifikation. Wir können solche Codes ebenfalls mit dem Schlüsselwort `MIT CODE` definieren:

```coffeescript
DANN Sie müssen mindestens 18 Jahre alt sein MIT CODE 4711
```



## Alternativer Regelausdruck

Die Grammatik von openVALIDATION erlaubt es, Regeln auf mehrere Weisen zu formulieren. Diese Flexibilität sorgt für einen noch höheren Grad an Natürlichkeit und löst sich damit weiter von einer restriktiven Syntax. Betrachten wir mal die Regel aus dem obigen Abschnitt:

```coffeescript
WENN Alter KLEINER 18
DANN Sie müssen mindestens 18 Jahre alt sein
```

Die alternative Ausdrucksweise für diese Regel sieht wie folgt aus:

```coffeescript
das Alter des Bewerbers DARF NICHT KLEINER als 18 sein    
```

Oder auch so:

```coffeescript
das Alter des Bewerbers MUSS MINDESTENS 18 sein
```

Der Vorteil bei der alternativen Variante ist, dass wir die Fehlermeldung nicht explizit angeben müssen. Die Regel selbst wird automatisch zu einer Fehlermeldung. Bei einem WENN / DANN Konstrukt liegt der Vorteil darin, dass wir die Fehlermeldung beliebig gestalten können.

Diese alternative Ausdrucksform muss einen sogenannten **Indikator** enthalten. Es gibt zwei Indikatoren: ein **MUSS** und ein **DARF NICHT**.

<table>
  <thead>
    <tr>
      <th style="text-align:left">SCHL&#xDC;SSELWORT</th>
      <th style="text-align:left">TYP</th>
      <th style="text-align:left">Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">DARF NICHT</td>
      <td style="text-align:left">Regel Indikator</td>
      <td style="text-align:left">Ein <code>DARF NICHT</code> Indikator identifiziert einen Ausdruck, als
        Regel.</td>
    </tr>
    <tr>
      <td style="text-align:left">MUSS</td>
      <td style="text-align:left">Regel Indikator</td>
      <td style="text-align:left">
        <p>Ein <code>MUSS </code>Indikator identifiziert einen Ausdruck, als Regel.</p>
        <p><em>Die Bedingung in so einem MUSS Ausdruck enth&#xE4;lt eine Implizite Negation!</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">KLEINER</td>
      <td style="text-align:left">Vergleichsoperator</td>
      <td style="text-align:left">Ein Vergleichsoperator &apos;&lt;&apos; f&#xFC;r numerische Operanden</td>
    </tr>
    <tr>
      <td style="text-align:left">MINDESTENS</td>
      <td style="text-align:left">Vergleichsoperator</td>
      <td style="text-align:left">Ein Alias f&#xFC;r den Vergleichsoperator &apos;&gt;=&apos;</td>
    </tr>
  </tbody>
</table>



## Regeln mit impliziten Bedingungen

Die Grammatik von openVALIDATION ermöglich die Erstellung von Regeln mit impliziten Bedingungen. Diese Fähigkeit vereinfacht die Erstellung und das Verständnis von Regeln. Wir haben folgendes Schema:

{% tabs %}
{% tab title="YAML" %}
```yaml
Unterschrieben: false
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    Unterschrieben: false
}
```
{% endtab %}
{% endtabs %}

Jetzt können wir folgende Regel erstellen:

```coffeescript
der Vertrag MUSS Unterschrieben sein
```

Wie man sieht enthält diese Regel lediglich das Attribut `Unterschrieben` aus dem Schema. Es fehlt quasi eine gültige Vergleichsoperation, bestehend aus zwei Operanden und einem Operator. Anhand des Datentypen ist openVALIDATION allerdings dazu in der Lage, den Ausdruck entsprechend zu interpretieren. Da es sich hier um einen Wahrheitswert handelt, orientiert sich openVALIDATION an der Semantik der natürlichen Sprache und macht einen impliziten Vergleich mit `true`:

`Unterschrieben ist GLEICH JA` bzw. durch das Konstrukt `MUSS` negiert `Unterschrieben ist GLEICH NEIN`.

Somit ergibt sich daraus automatisch folgende formale Regel:

```javascript
if(Unterschrieben != true)
    throw Error("der Vertrag MUSS unterschrieben sein")
```



## Regeln mit mehreren Bedingungen

Eine Regel kann unter Umständen mehrere miteinander verknüpfte Bedingungen enthalten:

```coffeescript
WENN das Alter des Bewerbers KLEINER 18 ist
 UND sein Wohnort ist NICHT Dortmund
DANN Sie müssen mindestens 18 Jahre alt sein und aus Dortmund kommen
```

Mit dem logischen Operator `UND`oder `ODER`können beliebig viele Bedingungen miteinander verknüpft werden. 

Das gleiche gilt auch für die alternative Ausdrucksform:

```coffeescript
    das Alter des Bewerbers DARF NICHT KLEINER 18 sein
UND sein Wohnort MUSS Dortmund sein
```



## Regeln mit verschachtelten Bedingungen

Um komplexe Regeln weiter zu strukturieren, kann Einrückung verwendet werden. Durch diese Einrückung wird der Zusammenhang und Priorität der Bedingungen festgelegt. 

```coffeescript
WENN das Alter des Bewerbers GRÖßER 18 ist
 UND der Name des Bewerbers ist GLEICH Mycroft Holmes
     ODER sein Name ist GLEICH Sherlock Holmes
DANN ist der Bewerber ein Genie
```

Der formale Ausdruck sieht dementsprechend folgendermaßen aus:

```javascript
Alter > 18 && (
    (Name == "Mycroft Holmes") || (Name == "Sherlock Holmes")
)
```

Die ODER Bedingung wird mit _**VIER**_ \(4x\) Leerzeichen eingerückt. Somit wird eine Klammerung mit der vorangegangenen Bedingung hergestellt. 

Solche Ausdrücke sind sehr komplex und können sowohl die Erstellung, als auch das Verständnis solcher Regeln enorm erschweren. Um diese zu vereinfachen, empfehlen wir solche komplexen Bedingungen in Teile zu zerlegen und diese in **Variablen** auszulagern. 

{% hint style="warning" %}
Normalerweise haben die Leerzeichen oder Einrückungen in der openVALIDATION Grammatik keine Bedeutung. Dies ist die einzige Stelle, an der die Anzahl der Leerzeichen eine Rolle spielt. Wir haben versucht die Grammatik so zu entwerfen, dass diese möglichst ohne kryptische Zeichen auskommt. Deshalb haben wir uns an dieser Stelle dazu entschieden, jeweils 4 Leerzeichen pro Verschachtelungstiefe als Indikator für die Klammerung der Bedingungen zu verwenden.
{% endhint %}



