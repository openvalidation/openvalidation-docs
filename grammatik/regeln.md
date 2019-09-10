# Rules

A rule is the most important construct in the grammar of openVALIDATION. They consist of a **condition** and an **action**. In a **validation rule**, the action is always an error message. The easiest way to express such a rule is a **IF / THEN** construct. If the condition is true, an error message is generated.

```coffeescript
  IF the age of the applicant is LESS than 18
THEN you must be at least 18 years old
```

The keywords must be written in capital letters so that they are clearly recognized as such. In the future, capitalization will be optional. 

| KEY WORD | TYPE | Description |
| :--- | :--- | :--- |
| IF | Rule | Selects the beginning of a rule and the next condition |
| THEN | Rule | Marks the beginning of an error message |
| LESS | Relational operator | A comparison operator '&lt;' for numeric operands |



## Condition

Let's take a closer look at the condition again:

```coffeescript
the age of the applicant is LESS than 18
```

This condition consists of a **comparison operation**. A comparison operation always has a **left** and a **right** operand and the corresponding **comparison operator**.  


## Relational operators

We have already got to know the comparison operator `LESS`. The comparison operators have different aliases. This enables us to express our rules as natural as possible. The following aliases already exist for the comparison operator `LESS`.

* SHORTER 
* LESS 
* LOWER 
* SMALLER
* FEWER

We could use one of these aliases to define the following expression, for example:

```coffeescript
the price is LOWER than 3 Euro
```

Over time this list will be completed and in the future it will be possible to add custom aliases.  


## Operands

The recognition of operands is the magic of our grammar. If we take a closer look at the content on the left and on the right side of the comparison operator, we see that it consists of many individual words. A person immediately understands that the attribute **age** and the number **18** are the relevant values. A machine, on the other hand, only sees a pile of words in a row.

`the age of the applicant` and`is 18`

Let us first look at the right operand: 

`is 18`

The resolution of this operand depends on the data type of the opposite left operand. If the left operand is a numeric value, the number 18 is automatically extracted from the right operand.  


## Semantical Sugar

The remaining word is irrelevant for machine processing and is simply ignored by openVALIDATION. Such content is called **semantical** or **grammatical sugar**. It makes it possible to express rules in an ornate and grammatically correct way. This makes the entire set of rules more readable for people and makes rules more intuitive to formulate.

So it depends on how the operand on the left side is recognized:

`the age of the applicant`

The relevant attribute is the age. openVALIDATION does not yet know this attribute. We have to make this known by specifying a schema in addition to a rule, which defines the corresponding data model with the respective attribute.

## Schema

The **schema** is essential for the resolution of the operands, since attributes in the operands of our rules are only recognized if they are also present in the schema. Let's look at the principle further at the operands`the age of the applicant`.

Let's consider the following highly simplified scheme, which only contains information about the Age attribute:

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

The schema can be passed as a JSON schema or in a simplified form as a JSON Object. Since the schema now contains an attribute called `Age`, the operand is recognized as such:

the `age`of the applicant

The remaining content of `the ... of the applicant` is declared as Semantical Sugar and ignored by openVALIDATION. The rule expression relevant for the machine looks like this after the resolution process:

```coffeescript
  IF age LESS 18
THEN you must be at least 18 years old
```

and here is the corresponding formal expression:

```javascript
if ( age < 18 ) 
   throw Error("You must be at least 18 years old.")
```



## Error message

The entire text behind the keyword **THEN** is recognized as an error message:

```coffeescript
You must be at least 18 years old.
```

Error messages often contain their own error codes for unique identification. We can also define such codes with the keyword `WITH CODE`:

```coffeescript
THEN you must be at least 18 years old WITH CODE 4711
```



## Alternative rule expression

The grammar of openVALIDATION is quite flexible. This flexibility partially cancels the formality and brings with it a certain naturalness. For this reason we have implemented an alternative expression for rules. We can express the above mentioned rule in the following way:

```coffeescript
the age of the applicant SHOULD NOT be LESS than 18    
```

or like that:

```coffeescript
the age of the applicant MUST be AT LEAST 18
```

The advantage of the alternative variant is that we do not have to explicitly specify the error message. The rule itself automatically becomes an error message. The advantage of a IF / THEN construct is that we can design the error message as we like.

This alternative form of expression must contain a so-called **indicator**. There are two indicators: one **MUST** and one **SHOULD NOT**.

| KEY WORD                                                                                  | TYPE | Description |
| :--- | :--- | :--- |
| SHOULD NOT | Rule indicator | A `SHOULD NOT` indicator identifies an expression, as a rule. |
| MUST | Rule indicator | A `MUST` indicator identifies an expression as a rule. _The condition in such a MUST expression contains an Implicit Negation!_ |
| LESS | Relational operator | A comparison operator '&lt;' for numeric operands |
| AT LEAST | Relational operator | An alias for the comparison operator '&gt;='. |



## Rules with implicit conditions

The grammar of openVALIDATION allows the creation of rules with implicit conditions. This ability simplifies the creation and understanding of rules. We have the following scheme:

{% tabs %}
{% tab title="YAML" %}
```yaml
signed : false
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
   signed : false
}
```
{% endtab %}
{% endtabs %}

Now we can create the following rule:

```coffeescript
the contract MUST be signed
```

As you can see, this rule only contains the `signed` attribute from the schema. A valid comparison operation is missing, consisting of two operands and one operator. However, openVALIDATION is able to interpret the expression accordingly on the basis of the data type. Since this is a boolean value, openVALIDATION is based on the semantics of natural language and makes an implicit comparison with `true`:

`signed EQUALS YES` or by the construct `MUST` negated `signed EQUALS NO`.

This automatically results in the following formal rule:

```javascript
if(signed != true)
    throw Error("the contract MUST be signed")
```



## Rules with multiple conditions

Under certain circumstances, a rule can contain several linked conditions:

```coffeescript
  IF the age of the applicant is LESS than 18
 AND his place of residence is NOT Dortmund
THEN You must be at least 18 years old and live in Dortmund.
```

With the logical operator `AND`or `OR`any number of conditions can be linked together.

The same applies to the alternative form of expression:

```coffeescript
    the age of the applicant SHOULD NOT be LESS than 18 years old
AND his place of residence MUST be Dortmund
```



## Rules with complex conditions

Indentation can be used to further structure complex rules. This indentation determines the context and priority of the conditions.

```coffeescript
  IF  the age of the applicant is GREATER 18
 AND  the name of the applicant IS Mycroft Holmes.
      OR his name IS Sherlock Holmes.
THEN  the applicant is a genius
```

Accordingly, the formal expression looks as follows:

```javascript
Age > 18 && (
    (Name == "Mycroft Holmes") || (Name == "Sherlock Holmes")
)
```

The `OR`condition is indented with **FOUR** \(4x\) spaces. This creates a parenthesis with the preceding condition.

Such expressions are very complex and can make the creation and understanding of such rules extremely difficult. To simplify them, we recommend to split such complex conditions into parts and to store them in **variables**.

{% hint style="warning" %}
Usually the spaces or indentations have no meaning in the openVALIDATION grammar. This is the only place where the number of spaces plays a role. We have tried to design the grammar in such a way that it gets by without cryptic characters. Therefore we decided to use 4 blanks per nesting depth as an indicator for the parenthesis of the conditions.
{% endhint %}



