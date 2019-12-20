# Variables

## Creating variables

Let us assume that our set of rules consists of the following two rules:

{% code title="" %}
```coffeescript

  IF the age of the applicant is LESS than 18
 AND his place of residence IS NOT Dortmund
THEN You must be at least 18 years old and live in Dortmund.


  IF the age of the applicant is LESS than 18
  OR his professional experience is SHORTER than 5 years
THEN You must be at least 18 years old and have work experience
     of at least 5 years

```
{% endcode %}

In both rules the query appears whether the user is already 18 years old. If we have 10, 20 or even 100 rules in which this query is needed, it becomes clear that such duplications unnecessarily inflate the clarity and complexity of the entire set of rules. 

At this point, variables provide a remedy. We can store the condition with the age in our own variable:

```coffeescript
   the age of the applicant is LESS than 18 years old
AS Underage
```

The variable has a variable indicator **AS**. Everything that precedes the `AS`is the value of a variable. The text after the keyword `AS`is its **name**.

Now that we have swapped the condition into a variable, we can use it in the appropriate places in our rules. And this is how our whole set of rules looks like:

{% code title="" %}
```coffeescript

     the age of the applicant is LESS than 18 years old
  AS underage

  IF the applicant is underage
 AND his residence IS NOT Dortmund
THEN You must be at least 18 years old and come from Dortmund.


  IF the applicant is underage
  OR his professional experience is LESS than 5 years
THEN You must be at least 18 years old and have work experience
     of at least 5 years
     
```
{% endcode %}

{% hint style="info" %}
Remember to always define variables before you use them. This means that the definition of a variable always needs to be anywhere above every place it is used in.
{% endhint %}

A further advantage when using variables is the additional semantics or domain specificity. We have packaged a complex and somewhat formal expression behind a simple semantically named variable. The one who creates the rules can influence the readability of his rules at this point.  


## Data type of a variable

Variables not only consist of a value and a name, they also have a data type. This data type is automatically derived from the value of a variable. Let's take another look at the upper condition:

```coffeescript
   the age of the applicant is LESS than 18 years old
AS underage
```

The value of this variable is a comparison operation. The result of a comparison operation is always true or false, i.e. a truth value. The data type of this variable is therefore **boolean**.

We can create variables with different data types, such as a **numeric** variable:

```coffeescript
   18
AS Number18
```

We can create variables with **arithmetic operations**

```coffeescript
   20 - 18
AS professional experience
```

Or a simple variable with a static text of type **String**

```coffeescript
   The fastest person in the world
AS Superman
```

For example, variables can contain the following schema attributes:

{% tabs %}
{% tab title="YAML" %}
```yaml
age: 0
residence : "text"
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    age: 0,
    residence : "text"
}
```
{% endtab %}
{% endtabs %}

And here is the access to the attributes of the **schema**:

```coffeescript
   Age - 18
AS professional experience

   Residence IS Cologne
AS Cologne Boy
```



{% hint style="info" %}
With the help of variables, complex rules can be simplified by outsourcing the frequently used or particularly complicated expressions. Thus, it is possible to give simple semantic names to complex formal expressions. This not only increases the readability of the rules, but also their maintainability.
{% endhint %}

