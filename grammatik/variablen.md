# Variablen

## Variablen erstellen

Nehmen wir an, unser Regelwerk besteht aus folgenden beiden Regeln:

{% code title="" %}
```coffeescript

WENN das Alter des Bewerbers KLEINER 18
 UND sein Wohnort NICHT Dortmund ist
DANN Sie dürfen nicht jünger als 18 Jahre alt sein und gleichzeitig 
     nicht aus Dortmund kommen


WENN das Alter des Bewerbers KLEINER 18 ist
ODER seine Berufserfahrung KÜRZER als 5 Jahre ist
DANN Sie müssen mindestens 18 Jahre alt sein und über eine Berufserfahrung
     von minimum 5 Jahren verfügen

```
{% endcode %}

In beiden Regeln kommt die Abfrage, ob der Benutzer bereits 18 Jahre alt ist, vor. Wenn wir 10, 20 oder gar 100 Regeln haben in denen diese Abfrage benötigt wird, wird deutlich, dass solche Dopplungen die Übersichtlichkeit und Komplexität des gesamten Regelwerks unnötig aufbläht. 

An dieser Stelle schaffen **Variablen** eine Abhilfe. Wir können die Bedingung mit dem Alter in eine eigene Variable auslagern:

```coffeescript
    das Alter des Bewerbers ist KLEINER 18
ALS Minderjährig
```

Die Variable hat einen Variablen-Indikator **ALS**. Alles was vor dem `ALS` steht ist der Wert einer Variablen. Der Text nach dem Schlüsselwort `ALS` ist ihr **Name**.

Jetzt, da wir die Bedingung in eine Variable ausgelagert haben, können wir diese an den entsprechenden Stellen in unseren Regeln verwenden. Und so sieht unser gesamtes Regelwerk aus:

{% code title="" %}
```coffeescript

     das Alter des Bewerbers ist KLEINER 18
ALS  Minderjährig


WENN der Bewerber Minderjährig
 UND sein Wohnort NICHT Dortmund ist
DANN Sie dürfen nicht jünger als 18 Jahre alt sein und gleichzeitig 
     nicht aus Dortmund kommen


WENN der Bewerber Minderjährig ist
ODER seine Berufserfahrung KÜRZER als 5 Jahre ist
DANN Sie müssen mindestens 18 Jahre alt sein und über eine Berufserfahrung
     von minimum 5 Jahren verfügen
     
```
{% endcode %}

{% hint style="info" %}
Bevor Variablen in Regeln oder anderen Variablen verwendet werden können, müssen sie definiert sein. Das bedeutet, dass Variablendefinitionen in der jeweiligen Datei immer "über" allen Stellen stehen müssen, an denen sie verwendet werden. 
{% endhint %}

Ein weiterer Vorteil bei der Verwendung von Variablen ist die zusätzliche Semantik bzw. die Domainspezifik. Wir haben einen komplexen und etwas formalen Ausdruck hinter einer einfachen semantisch benannten Variable verpackt. Derjenige, der die Regeln erstellt kann an dieser Stelle einen Einfluss auf die Lesbarkeit seiner Regeln nehmen.   


## Datentyp einer Variablen

Variablen bestehen nicht nur aus einem Wert und einem Namen, sie besitzen auch einen Datentypen. Dieser Datentyp wird automatisch aus dem Wert einer Variablen abgeleitet. Schauen wir uns nochmal die obere Bedingung an:

```coffeescript
    das Alter des Bewerbers ist KLEINER 18
ALS Minderjährig
```

Der Wert dieser Variablen ist eine Vergleichsoperation. Das Ergebnis einer Vergleichsoperation ist immer Wahr oder Falsch, also ein Wahrheits-Wert. Der Datentyp dieser Variablen ist demnach ein **boolean**.

Wir können Variablen mit verschiedenen Datentypen erstellen wie beispielsweise eine numerische Variable:

```coffeescript
    18
ALS Zahl18
```

Wir können Variablen mit **arithmetischen Operationen** erstellen

```coffeescript
    20 - 18
ALS Berufserfahrung
```

Oder eine einfache Variable mit einem statischen Text vom Typ **String**

```coffeescript
    Der schnellste Mensch der Welt
ALS Superman
```

Variablen können beispielsweise folgende Schema-Attribute enthalten:

{% tabs %}
{% tab title="YAML" %}
```yaml
Alter: 0
Wohnort: Text
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    Alter: 0
    Wohnort: "Text"
}
```
{% endtab %}
{% endtabs %}

Und hier ist der Zugriff auf die Attribute des **Schemas:**

```coffeescript
    Alter - 18
ALS Berufserfahrung

    Wohnort IST Köln
ALS Kölsche Jung
```



{% hint style="info" %}
Mit Hilfe der Variablen können komplexe Regeln vereinfacht werden, indem man die häufig verwendeten oder besonders komplizierten Ausdrücke auslagert. So ist es möglich komplexen, formalen Ausdrücken einfache, semantische Namen zu verleihen. Dadurch erhöht sich nicht nur die Lesbarkeit der Regeln, sondern auch die Wartbarkeit.
{% endhint %}

