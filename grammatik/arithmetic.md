# Arithmetic

The grammar supports arithmetic and mathematical operations:

```coffeescript
    20 - 18
AS professional experience
```

Of course, variables or attributes from the schema can also be used within an arithmetic operation.

Let's look at the following scheme and two predefined variables:

{% tabs %}
{% tab title="YAML" %}
```yaml
age : 0
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
   age : 0
}
```
{% endtab %}
{% endtabs %}

```coffeescript
    18
AS career start

    age - career start
AS professional experience
```

Arithmetic expressions can be arbitrarily complex using parentheses:

```coffeescript
    (20 - 18) * 12
AS professional experience in months
```

Arithmetic expressions can also be used directly within a rule:

```coffeescript
    the professional experience from age - 18 years MUST NOT be LESS than 10 
```



## Reference

Currently the following mathematical operations are supported:

| Operator | Name | Discription |
| :--- | :--- | :--- |
| + | Addition | simple addition |
| - | Subtraction | simple subtraction |
| \* | Multiplication | simple multiplication |
| / | Division | simple division |
| MOD | Modulo | remaining  |
|  |  | more will be added soon... |

