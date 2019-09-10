---
description: coming soon . . .
---

# Examples

## Conditions

### IF - THEN

\*\*\*\*[OpenValidation ](https://docs.openvalidation.io/)defines conditions and actions of each rule with **IF - THEN**  terms. Every rule has to have at least one keyword. 

{% tabs %}
{% tab title="Rule" %}
```typescript
IF the equity of the person is LESS than 30000€
THEN the loan may not be grantedF 
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    equity: 25000
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```typescript
IF the company's location IS NOT in Munich
THEN business is getting harder
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    location: Dortmund
}
```
{% endtab %}
{% endtabs %}

### Alternative rule expressions

{% hint style="info" %}
Alternative expressions are containing the [indicator-keywords](https://docs.openvalidation.io/grammatik/kaywords#implizite-bedingungen-alternativer-regelausdruck). The condition is an implicit negation.
{% endhint %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
the applicants age HAS NOT to be LESSER than 18
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    age: 17
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
the applicants age HAS to be AT LEAST 18
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    age: 17
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
the persons location SHOULD be germany
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    location: iceland
}
```
{% endtab %}
{% endtabs %}

### Implicit condition

{% hint style="info" %}
You can use implicit conditions for boolean expressions .
{% endhint %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
the contract HAS to be signed
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    signed: false
}
```
{% endtab %}
{% endtabs %}

### Multiple conditions

#### AND

{% tabs %}
{% tab title="Rule" %}
```coffeescript
  IF the age of the applicant is GREATER than 70
 AND his residence is NOT Dortmund
THEN You are not allowed to be older than 70 and you must come from Dortmund
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    residence: "Berlin",
    age: 18
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
     the applicant's age SHOULD NOT BE LESS than 25 years old
 AND his profession MUST be a consultant

```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
 {
    profession: "Developer",
    age: 18
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
    the applicant's degree MUST be a Bachelor
AND his age MUST be at LEAST 25 years
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    age: 24,
    degree: "Master"
}
```
{% endtab %}
{% endtabs %}

#### OR

{% tabs %}
{% tab title="Rule" %}
```coffeescript
  IF the degree of applicant IS NOT Bachelor
  OR his professional experience is LESS than 5 years
THEN you must have a bachelor's degree or 5 years experience
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    experience: 6,
    degree: "Master"
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
   the applicant's degree MUST be a Bachelor
OR his professional experience MUST be at LEAST 5 years
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    experience: 6,
    degree: ""
}
```
{% endtab %}
{% endtabs %}

### Complex condition

{% hint style="warning" %}
You have to pay attention to indents for complex conditions
{% endhint %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
  IF the place of residence of the person IS London
 AND the name of the person EQUALS Mycroft Holmes.
     OR his name EQUALS Sherlock Holmes.
THEN the person is a genius
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    residence: "London",
    name: "Sherlock Holmes"
}
```
{% endtab %}
{% endtabs %}

## Arithmetic examples

{% tabs %}
{% tab title="Rule" %}
```coffeescript
professional experience from age - 18 years MUST NOT be LESS than 10 years
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    age: 25
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
   IF assets - liabilities NOT EQUAL 0 
 THEN the balance must always be balanced
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    assets: 1,
    liabilties: 2
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Rule" %}
```coffeescript
  IF equity / total assets NOT SAME equity ratio
THEN the balance sheet contains errors 
```
{% endtab %}

{% tab title="Schema" %}
```coffeescript
{
    assets: 100,
    equity: 25
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
String operations are not possible at the moment
{% endhint %}

## Variables

Numeric values

{% tabs %}
{% tab title="Variable definition" %}
```coffeescript
   42
AS answer to everything
```
{% endtab %}
{% endtabs %}

Strings

{% tabs %}
{% tab title="Variable definition" %}
```coffeescript
   Validaria
AS helpful
```
{% endtab %}
{% endtabs %}

Boolean expressions

{% tabs %}
{% tab title="Variable definition" %}
```coffeescript
   the age of the applicant is LESS than 18 years old
AS underage
```
{% endtab %}
{% endtabs %}

Arithmetic expressions

{% tabs %}
{% tab title="Variable definition" %}
```coffeescript
   age - 18
AS professional experience
```
{% endtab %}
{% endtabs %}

## Comments

{% tabs %}
{% tab title="comment" %}
```coffeescript
COMMENT This is a commentary
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="comment" %}
```coffeescript
COMMENT Multi-line comments
        are also possible
```
{% endtab %}
{% endtabs %}

## OpenAPI

[OpenVALIDATION ](https://docs.openvalidation.io/)ships a OpenAPI plugin, which defines the validationrules for the serviceoperations.

{% tabs %}
{% tab title="oapi.spec.yaml" %}
```coffeescript
openapi: "3.0.0"
info:
  version: 1.0.0
  title: openVALIDATION
  description: openVALIDATION Beispiele.
paths:
  /:
   post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/person'
            x-ov-rules:
              culture: en
              rule: |

                  COMMENT This is a comment, Miss Honey
              
                  IF the persons equity is LESS THAN 30000 €
                  THEN a loan could not be granted
                  
                  
     responses:
        '200':
          description: success
components:
  schemas:
    person:
      type: object
      properties:
        equity:
          type: integer   
```
{% endtab %}

{% tab title="Download" %}
{% file src=".gitbook/assets/oapi.spec.yaml" caption="oapi.spec.yaml" %}
{% endtab %}
{% endtabs %}

