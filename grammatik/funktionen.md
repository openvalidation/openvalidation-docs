# Functions

openVALIDATION provides various functions. Let's say we want to check in a rule whether we have spent more money on shopping than our budget allows.

We have the following scheme:

{% tabs %}
{% tab title="YAML" %}
```yaml
shopping_list: 
    - article : text
      price : 0
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
	shopping_list: [
		{
			article : "text",
			price : 0
		}
	]
}
```
{% endtab %}
{% endtabs %}

And here's the rule that goes with it:

```coffeescript
SUM OF shopping_list.price
    AS expenses

the expenses MUST NOT EXCEED the budget of 20 €.
```

First, we define a variable called **Expenses**. This variable uses the **SUM OF** function. The list of all **prices** from the **shopping list** is transferred to the function. The function **SUM OF** calculates a sum and thus has a return value of type **Number**. The Issues variable is automatically assigned this data type and can then be used within the rule.

The rule itself contains a numeric comparison operation. The whole thing looks like this:

```javascript
var expenses = sum(shopping_list.map(a -> a.price))

If (expenses > 20 )
   throw Error("the expenses MUST NOT EXCEED the budget of 20 €")
```

Functions can of course also be used within a rule.  


