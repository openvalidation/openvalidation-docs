# Arithmetik

Die Grammatik unterstützt arithmetische bzw. mathematische Operationen:

```coffeescript
    20 - 18
ALS Berufserfahrung
```

Selbstverständlich können auch Variablen oder Attribute aus dem Schema innerhalb einer arithmetischen Operation verwendet werden.

Betrachten wir folgendes Schema und zwei vordefinierte Variablen:

{% tabs %}
{% tab title="YAML" %}
```yaml
Alter : 0
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
   Alter : 0
}
```
{% endtab %}
{% endtabs %}

```coffeescript
    18
ALS Berufseinstieg

    Alter - Berufseinstieg
ALS Berufserfahrung
```

Arithmetische Ausdrücke können beliebig komplex sein, indem üblicherweise Klammerung verwendet wird:

```coffeescript
    ( 20 - 18 ) * 12
ALS Berufserfahrung in Monaten
```

Arithmetische Ausdrücke können auch direkt innerhalb einer Regel verwendet werden:

```coffeescript
    die Berufserfahrung von Alter - 18 Jahren DARF NICHT GERINGER sein als 10 
```



## Referenz

Aktuell werden folgende mathematische Operationen unterstützt:

| Operator | Name | Beschreibung |
| :--- | :--- | :--- |
| + | Addition | einfache Addition |
| - | Subtraktion | einfache Subtraktion |
| \* | Multiplikation | einfache Multiplikation |
| / | Division | einfache Division |
| MOD | Modulo | Rest  |
|  |  | weitere folgen in Kürze... |

