# Kommentare

Man kann in dem Regelwerk auch Kommentare verfassen. Diese enthalten keinerlei Logik. Sie dienen lediglich zur Informationszwecken und werden im generierten Code nachher ebenfalls als Kommentare generiert. Ein Kommentar beginnt immer mit dem Schlüsselwort `KOMMENTAR`.

```coffeescript
KOMMENTAR Dies ist ein Kommentar
```

Kommentare können auch für Kollaboration an Regelsätzen verwendet werden, um Änderungsvorschläge zu dokumentieren.

Selbstverständlich können auch mehrzeilige Kommentare verfasst werden.

```coffeescript
KOMMENTAR Dies ist ein Kommentar
          Und hier auch...
```

{% hint style="warning" %}
Bei mehrzeiligen Kommentaren ist zu beachten, dass unter keinen Umständen zwei aufeinanderfolgende Zeilenumbrüche verwendet werden sollten.
{% endhint %}

