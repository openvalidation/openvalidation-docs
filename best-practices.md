# Best practice

In order for the creation of validation rules in openVALIDATION to be as simple and efficient as possible, a lot should be considered.

![](.gitbook/assets/image%20%2838%29.png)

## Setting up openVALIDATION

The target group, which is primarily concerned with the creation of the rules, are experts such as:

* Domain Experts
* Business Analysts
* Requirement engineers
* In special use cases, it can also be the end users themselves

In order to start creating the validation rules, the CLI and the generated code must first be set up or integrated into the development environment by a technician, preferably a developer.

After a successful setup phase, the professionals should create the rules in an environment isolated from the technology. All you need is any text editor to edit and save the rules.

The entire set of rules should be stored in one or more files with the extension **\*.ov**.



## The structure of the rule set

The entire set of rules usually consists of several validation rules and other additional structural elements, such as:

* Variables
* IMPORTS
* COMMENTS
* Domain-specific operators \(in progress...\)

They all serve one purpose. They keep the complexity in the fence and thus simplify the creation or modification of validation rules.

The set of rules should be structured in such a way that the technical complexity is abstracted as far as possible from the natural definition of a rule.

In software development, but also in the world of technology as a whole, it is common to divide complexity into several layers. The lower layer, the technical layer, is usually referred to as the low level. The higher layer, which is abstracted from the technology, is called high Level.  



## Creation of rules

The set of rules in openVALIDATION should follow this fundamental principle. The top layer of the set of rules are the rules themselves. The rules should therefore be formulated as naturally as possible.

The rules are **atomic**. They should not contain any dependencies to other rules. The order of the rules is also irrelevant.

```coffeescript
the age of the applicant MUST NOT be LESS than 18 

the location of the applicant MUST be Dortmund
```



## Preconditions

Preconditions can be defined in the form of variables. For example, a frequently used condition is whether the applicant is already 18 years old. Can be outsourced behind a simple variable called underage. This variable can also be used as a precondition in various rules.

```coffeescript
     the age of the applicant is LESS than 18 years old
  AS underage

     the applicant MUST NOT be underage
 AND his location MUST be Dortmund

  IF the applicant is underage
  OR his professional experience is SHORTER than two years.
THEN you lack of experience   
```

This reduces the complexity and increases the readability of the actual rules. 

{% hint style="info" %}
Basically, the rule applies if a condition is used in several rules, then it is a precondition or subcondition and should be stored in a reusable variable.
{% endhint %}



## Names of the variables

Variables are very useful constructs. They can greatly increase the readability and naturalness of a validation rule. Even if spaces and other special characters are allowed in the name of a variable, you should still choose a name that is as meaningful and flexible as possible.

```coffeescript
   The professional experience of the applicant is LONGER than 10 years.
AS senior

the applicant MUST be a senior

the applicant MUST NOT be a senior

```





