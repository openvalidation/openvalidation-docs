---
description: in Arbeit...
---

# Domainspezifische Operatoren

Das Ziel der Grammatik von openVALIDATION ist es, so natürlich wie möglich zu sein. Um diesem Ziel noch näher zu kommen, gibt es die Möglichkeit domänenspezifische bzw. semantische Operatoren im Regelwerk zu deklarieren. 

**Ein Beispiel**

Schema:

{% tabs %}
{% tab title="YAML" %}
```yaml
Alter: 0
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

Regel:

```coffeescript
das Alter des Bewerbers MUSS MINDESTENS 18 sein
```

Die Regel klingt schon recht natürlich, aber nicht ganz. Viel natürlicher könnte die Regel klingen, wenn man diese folgendermaßen ausdrücken würde:

```coffeescript
der Bewerber DARF NICHT JÜNGER als 18 sein
```

Das klingt schon viel natürlicher. Das Problem ist, dass selbst, wenn openVALIDATION einen Alias für den Operator `<` namens `JÜNGER` bereitstellen würde, weiß openVALIDATION am Ende nicht, welches Attribut des Schemas für die Vergleichsoperation verwendet werden soll. Das müssen wir openVALIDATION irgendwie mitteilen. Das machen wir, indem wir einen benutzerdefinierten, domänenspezifischen Operator definieren:

```coffeescript
 OPERAND  Alter
OPERATOR  KLEINER
     ALS  JÜNGER

der Bewerber DARF NICHT JÜNGER als 18 sein
```

Wir können mit Hilfe eines neuen globalen Elements **OPERATOR** einen domänenspezifischen Vergleichsoperator erstellen. Diesen können wir anschließend in unseren Regeln verwenden. In unserem Beispiel haben wir einen neuen Vergleichsoperator namens `JÜNGER` erstellt. Der Operator selbst besteht aus einem konkreten Vergleichsoperator, nämlich `KLEINER` oder `<` und dem jeweiligem Attribut des Schemas.

{% hint style="info" %}
Domänenspezifische Operatoren können im Vorfeld beispielsweise zusammen mit den Variablen erstellt werden und dienen anschließend als semantische Grundlage für die eigentlichen Validierungsregeln.
{% endhint %}



