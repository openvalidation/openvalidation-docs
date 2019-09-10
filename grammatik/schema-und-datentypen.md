# Schema and Data types

One of the most important features of openVALIDATION is the recognition of data structures. The schema is essential for the creation of rules and can enormously increase the complexity of the entire set of rules due to its own complexity. When working with very complex schemas, there is a lot to consider in order to make the set of rules as simple and handy as possible.

## Easy access

There are various ways of accessing the attributes of the schema. In principle, an attribute of the schema can be used both within a **rule** and within a **variable**. Let's look at the following schema:

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

And the following rule:

```coffeescript
   the age of the applicant is LESS than 18 years old
AS Underage

the age of the applicant MUST be at LEAST 18
```



## Access to attributes of an object

As a rule, a scheme is anything but flat. It consists of many complex objects. Such complex structures can be resolved automatically by openVALIDATION.

{% tabs %}
{% tab title="YAML" %}
```yaml
delivery_address: 
    location : text
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
	delivery_address: {
		location : "text"
 	}
}
```
{% endtab %}
{% endtabs %}

```coffeescript
the location of the customer MUST be Dortmund
```

The corresponding formal expression then looks like this:

```javascript
if ( delivery_address.location != "Dortmund" )
    throw Error("the location of the customer MUST be Dortmund")
```

openVALIDATION automatically recognizes that the attribute Location is an attribute of the subobject Delivery Address.

{% hint style="warning" %}
However, this resolution only works as long as the attribute only occurs once in the entire schema!
{% endhint %}

We extend the schema as follows:

{% tabs %}
{% tab title="YAML" %}
```yaml
delivery_address: 
	location : text
	
billing_address: 
	location : text
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
	delivery_address: {
		location : "text"
 	},
	billing_address: {
		location : "text"
 	}
}
```
{% endtab %}
{% endtabs %}

Now, if we use the same rule that we used before:

```coffeescript
the location of the customer MUST be Dortmund
```

This poses a problem. openVALIDATION no longer knows which of the two existing attributes name **location** is meant. This has to be specified a little bit. We can do this by using the structural access to the concrete attribute Location:

```coffeescript
the delivery_address.location of the customer MUST be Dortmund
```

That works, but the rule doesn't look very natural. We can move the **location** of the **delivery address** to a separate variable to restore the semantics of the rules:

```coffeescript
delivery_address.location AS delivery_location

the delivery_location of the customer MUST be Dortmund
```

Our rule has already become more readable again.

{% hint style="info" %}
So we can use variables to simplify complex access to schema attributes while increasing the readability of the rules.
{% endhint %}



## Access to list elements/ arrays

Especially the access to the list elements or arrays can become quite complex under certain circumstances.

The following scheme is available:

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

Suppose we have a shopping list and want to keep the prices of all items in a list.

```coffeescript
shopping_list.price AS prices
```

openVALIDATION recognizes from the schema that the attribute price is a part of the complex type, which in turn is located in an array called shopping list. Based on this information and the realization that the attribute price is a numeric value, the following formal variable is created:

```javascript
Number[] prices = shopping_list.map(a -> a.price)
```

