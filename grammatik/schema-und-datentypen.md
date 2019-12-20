# Schema und Datentypen

Eine der wichtigsten Fähigkeiten von openVALIDATION ist die Erkennung von Datenstrukturen. Das Schema ist essentiell für die Erstellung von Regeln und kann aufgrund der eigenen Komplexität die Komplexität des gesamten Regelwerkes enorm erhöhen. Beim Arbeiten mit sehr komplexen Schemas gibt es einiges zu beachten, um das Regelwerk so einfach und handlich wie möglich zu gestalten.

## Einfacher Zugriff

Es gibt verschiedene Zugriffsmöglichkeiten auf die Attribute des Schemas. Grundsätzlich kann ein Attribut des Schemas sowohl innerhalb einer **Regel**, als auch innerhalb einer **Variablen** verwenden werden. Betrachten wir das folgende Schema:

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

Und die folgende Regel:

```coffeescript
    das Alter des Bewerbers ist KLEINER 18
ALS Minderjährig

das Alter des Bewerbers MUSS MINDESTENS 18 sein
```



## Zugriff auf Attribute eines Objektes

In der Regel ist ein Schema alles andere als flach. Es besteht aus vielen verschachtelten Objekten. Solche verschachtelten Strukturen kann openVALIDATION automatisch auflösen.

{% tabs %}
{% tab title="YAML" %}
```yaml
Lieferadresse:
    Ort: Text
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
	Lieferadresse: {
		Ort : "Text"
 	}
}
```
{% endtab %}
{% endtabs %}

```coffeescript
der Ort des Kunden MUSS Dortmund sein
```

Der entsprechende formale Ausdruck sieht dann folgendermaßen aus:

```javascript
if ( Lieferadresse.Ort != "Dortmund" )
    throw Error("der Ort des Kunden MUSS Dortmund sein")
```

openVALIDATION erkennt automatisch, dass das Attribut **Ort** ein Attribut des Unterobjekts **Lieferaddresse** ist.

{% hint style="warning" %}
Diese Auflösung funktioniert allerdings nur solange das Attribut im gesamten Schema nur einmal vorkommt!
{% endhint %}

Wir erweitern das Schema folgendermaßen:

{% tabs %}
{% tab title="YAML" %}
```yaml
Lieferadresse:
	Ort : Text
	
Rechnungsadresse:
	Ort : Text
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
	Lieferadresse: {
		Ort : "Text"
 	},
	Rechnungsadresse: {
		Ort : "Text"
 	}
}
```
{% endtab %}
{% endtabs %}

Wenn wir jetzt dieselbe Regel verwenden wie vorhin:

```coffeescript
der Ort des Kunden MUSS Dortmund sein
```

Dann ergibt sich ein Problem. openVALIDATION weiß nicht mehr welcher der beiden vorhandenen Attribute Namens **Ort** gemeint ist. Das muss etwas präzisiert werden. Dies können wir tun, indem wir den strukturellen Zugriff auf das konkrete Attribut Ort verwenden:

```coffeescript
der Lieferadresse.Ort des Kunden MUSS Dortmund sein
```

Das funktioniert zwar, allerdings sieht die Regel nicht sehr natürlich aus. Wir können den **Ort** der **Lieferadresse** in eine separate Variable auslagern, um die Semantik der Regeln wiederherzustellen:

```coffeescript
Lieferadresse.Ort ALS Lieferort

der Lieferort des Kunden MUSS Dortmund sein
```

Schon ist unsere Regel wieder lesbarer geworden. 

{% hint style="info" %}
Wir können also Variablen nutzen, um komplexe Zugriffe auf Attribute des Schemas zu vereinfachen und gleichzeitig die Lesbarkeit der Regeln zu erhöhen.
{% endhint %}



## Zugriff auf Listenelemente/ Arrays

Besonders der Zugriff auf die Listenelemente bzw. Arrays kann unter Umständen recht komplex werden.

Folgendes Schema liegt vor:

{% tabs %}
{% tab title="YAML" %}
```yaml
Einkaufsliste:
    - Artikel: Text
      Preis: 0
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
   Einkaufsliste: [
     {
       Artikel: "Text",
       Preis: 0
     }
  ]
}
```
{% endtab %}
{% endtabs %}

Angenommen, wir haben eine Einkaufsliste und möchten die Preise aller Artikel in einer Liste vorhalten. 

```coffeescript
Einkaufsliste.Preis ALS Preise
```

openVALIDATION erkennt anhand des Schemas, dass das Attribut Preis ein Teil des Komplextypes ist, der sich wiederum in einem Array namens Einkaufsliste befindet. Anhand dieser Informationen und der Erkenntnis, dass das Attribut Preis ein numerischer Wert ist, entsteht folgende formale Variable:

```javascript
Number[] Preise = Einkaufsliste.map(a -> a.Preis)
```

