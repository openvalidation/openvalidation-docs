# Structuring the set of rules

IT projects usually contain more than one validation rule. Depending on the project size, a project can contain several 1000 rules. The complexity of the rules must also be kept in check. A good way to reduce complexity is a reasonable modularization concept. It is about dividing something big into small, modular components.

The following guidelines exist for structuring the entire set of rules. Some of them are mandatory and are therefore an integral part of the grammar. The others are optional and serve as recommendations.

{% hint style="info" %}
The first recommendation is to save the entire set of rules in a file with the extension **\*.ov**.
{% endhint %}



## Global elements

A set of rules can consist of several rules:

```coffeescript
the contract MUST be signed

the age of the applicant MUST be at LEAST 18
```

We call rules, variables and comments **global elements**. These must always be separated from each other by a **paragraph**, whereby paragraphs consist of at least two line breaks without content.

```coffeescript
     the age of the applicant is LESS than 18 years.
  AS underage

  IF the applicant is underage
 AND his place of residence is NOT Dortmund
THEN You must be at least 18 years old and come from Dortmund.


COMMENT Above me is a multiline paragraph
```

The upper set of rules consists of three global elements:

* a variable ,
* a validation rule,
* and a comment

The separation of the Global Elements by a paragraph is mandatory and is therefore part of the grammar. In this way you can write as many rules, variables and comments as you like. It is recommended to write down all variables in the upper part of the set of rules.  


## Formatting complex rules

For a complex **IF/ THEN** rule to remain readable, it should be formatted as follows:

```coffeescript
  IF the applicant is underage
 AND his place of residence is NOT Dortmund
THEN You must be at least 18 years old and come from Dortmund.
```



* Within a rule, the conditions and the error message are to be separated by a line break
* The keywords, such as `IF`, `AND` and `THEN`, should be right-aligned with the longest keyword
* After the first keyword of the respective line there are at least two spaces \(the number can vary. Consistency is important\).
* Long conditions or error messages can be formatted again with a line break. It is important that the next line is flush with the beginning of the previous line, as is the case with the error message that comes after the `THEN` keyword and contains a line break



## Formatting variables

```coffeescript
    the age of the applicant is LESS than 18 years old
 AS underage
```

The keyword `AS` and the name should be separated from the actual value of the variable by a line break. The value of the variable should be flush with the name of the variable. This formatting is optional and only serves to improve readability.

## Splitting the set of rules into several files

It is possible to split the entire set of rules into several files. For example, you can store the variables in a separate file and add external files using the keyword `IMPORT`:

{% code title="variables.ov" %}
```coffeescript
    the age of the applicant is LESS than 18 years old
 AS underage
```
{% endcode %}

{% code title="rules.ov" %}
```coffeescript
IMPORT ./variables.ov

  IF the applicant is underage
 AND his place of residence is NOT Dortmund
THEN You must be at least 18 years old and come from Dortmund.
```
{% endcode %}

