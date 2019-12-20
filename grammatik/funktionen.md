# Funktionen

openVALIDATION stellt verschiedene Funktionen bereit. Mal angenommen, wir möchten in einer Regel prüfen, ob wir beim Einkaufen mehr Geld ausgegeben haben, als unser Budget es zulässt.

Wir haben folgendes Schema:

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

Und hier ist die passende Regel dazu:

```coffeescript
    SUMME VON Einkaufsliste.Preis
ALS Ausgaben

die Ausgaben DÜRFEN NICHT das Budget von 20 € ÜBERSTEIGEN
```

Zunächst definieren wir eine Variable namens **Ausgaben**. In dieser Variablen wird die Funktion **SUMME VON** verwendet. An die Funktion wird die Liste aller **Preise** aus der **Einkaufsliste** übergeben. Die Funktion SUMME VON errechnet eine Summe und hat somit eine Rückgabewert vom Typ **Number**. Die Variable Ausgaben bekommt automatisch diesen Datentyp zugeordnet und kann anschließend innerhalb der Regel verwendet werden.

Die Regel selbst enthält eine numerische Vergleichsoperation. Das Ganze sieht formal folgendermaßen aus:

```javascript
var Ausgaben = sum(Einkaufsliste.map(a -> a.Preis))

If (Ausgaben > 20 )
   throw Error("die Ausgaben DÜRFEN NICHT das Budget von 20 € ÜBERSTEIGEN")
```

Funktionen können selbstverständlich ebenfalls innerhalb einer Regel verwendet werden.  


