---
description: in process...
---

# Domain-specific operators

The aim of openVALIDATION's grammar is to be as natural as possible. To get even closer to this goal, it is possible to declare domain-specific or semantic operators in the rules.

**An example**

Schema:

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

Rule:

```coffeescript
the age of the applicant MUST be AT LEAST 18
```

The rule sounds quite natural, but not quite. The rule could sound much more natural if it were expressed as follows:

```coffeescript
the applicant MUST NOT be YOUNGER than 18
```

That sounds a lot more natural. The problem is that even if openVALIDATION would provide an alias for the operator `<` called `YOUNGER`, at the end openVALIDATION does not know which attribute of the schema should be used for the comparison operation. We have to tell openVALIDATION that somehow. We do this by defining a user-defined, domain-specific operator:

```coffeescript
age and smaller
as younger

the applicant must not be younger than 18
```

We can create a domain-specific comparison operator using a new global element **as operator**. We can then use this in our rules. In our example, we have created a new comparison operator called **`younger`**. The operator itself consists of a concrete comparison operator, namely **`less`** and the attribute **age** of the schema. The keyword **and** in between is optional. 

{% hint style="info" %}
Domain-specific or semantic operators can be created in advance, e.g. together with the variables, and then serve as a semantic basis for the actual validation rules.
{% endhint %}



