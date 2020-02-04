---
description: Description of the basic architecture
---

# Architecture

At this point the core functionality and the corresponding architecture of **openVALIDATION** is described. This description does not include the CLI or REST component. A natural language rule and the corresponding schema are expected as input parameters. Afterwards the processing starts, which can be separated into 5 subroutines \(preprocessor, schema converter, parser, validation, generator\). At the end of the compilation process program code is generated.

![simplified view of the compilation process](../../.gitbook/assets/image%20%2831%29.png)

The preprocessor prepares the natural language rule. At this point, for example, a translation or normalization of the keywords \(aliases\) takes place. The parser generates the Abstract Syntax Tree \(AST\). The AST is the logical structure of the grammar, which in our case represents the domain of the validation rules. The AST is then processed further with the help of the generator, so that valid program code is generated at the end of the entire processing procedure.



### Rule

openVALIDATION makes it possible to describe validation rules in a natural language. Strictly speaking, there are many different languages such as **German** or **English**. The languages are extensible, so you can add more languages like **Spanish** or **French** with a manageable effort.

In order for openVALIDATION to extract a valid validation rule from a raw text the special keywords must be included in this text. There is a set of fixed keywords. Each of these keywords has one or more aliases in the respective language. For example, the keyword **should not** has additional aliases such as: **must not** or **have not** . Each keyword has a normalized value for which several aliases exist in each natural language. The configuration of these aliases can be found in the [Resources](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-core/src/main/resources) folder of the [openvalidation-core](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-core) project.

Here is an excerpt from [aliases\_en.properties](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/resources/aliases_en.properties) where the English language aliases are defined.

```typescript
...

ʬoperatorʬequals = IS, EQUAL, EQUALS, IS EQUAL TO
ʬoperatorʬnot_equals = ISN'T, IS NOT, NOT EQUAL, NOT EQUALS, NOT

...
```

Die Auflösung bzw. die Normalisierung dieser Aliases erfolgt im Preprocessor. 

Any text can be placed around the keywords. We call it sematic sugar. Thanks to this **semantic sugar**, the creator can make his rules very flexible and describe the set of rules in much more detail. Other editors can better understand the rules while the machine simply ignores the semantic sugar. This flexibility is the core of the grammar of openVALIDATION. It makes it so natural compared to other DSL's.

For example, it would be quite sufficient to describe the rule in this way. For the compiler this would be enough:

```typescript
age must less 18
```

However, it is not so nice for humans to read. The age of who? 18 what? For human understanding you need a little more context:

```csharp
applicant's age should not be less than 18 years
```

It sounds much more natural. The larger and more complex the set of rules is, the more useful the ability of the semantic context proves to be. Another advantage arises when you dictate the rule instead of writing it. Thus, voice assistants can be equipped with the ability to generate code or to store rules.



### Schema

A validation rule always requires a data model or schema so that it knows which data is to be validated. Usually a data model already exists before you start to create validation rules. Therefore openVALIDATION compiler expects the corresponding schema in addition to the actual set of rules.

{% hint style="warning" %}
It is not the task of openVALIDATION to create a data model or a schema. The schema must be passed to openVALIDATION as input parameter.
{% endhint %}

The schema must be currently defined in [JSON Schema](https://json-schema.org/) format. To simplify the use of openVALIDATION, the schema can also be defined in  [JSON](https://www.json.org/) Object Format. From this a possible JSON schema is automatically derived which is then further processed.

Here is an example of a possible schema:

```javascript
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "age": {
      "type": "number",
    }
  }    
}
```

The schema actually defines a single attribute named **age** and of **type** **number**. It is quite complex if you just want to fire a CLI command or just try it out. Therefore openVALIDATION offers the possibility to specify the schema in a simple **JSON object format**:

```javascript
{
    age : 0
}
```

This is a simple way to pass the same information to the compiler. An attribute **age** that contains a value of **0**. You can also pass another value, but the value will be ignored. It is only used to determine the **data type** of the attribute.

{% hint style="info" %}
If you want to realize a deeper integration of openVALIDATION into other systems it is always recommended to use JSON Schema instead of JSON Object. This allows a schema to be specified much more precisely.
{% endhint %}



### Schema converter

xxx

![](../../.gitbook/assets/image%20%2812%29.png)



### Preprocessor

xxx

  


![The internal design of the preprocessor](../../.gitbook/assets/image%20%2810%29.png)

xx



### Parser

xxx

![](../../.gitbook/assets/image%20%2843%29.png)



### Abstract Syntax Tree \(AST\)

The Abstract Syntax Tree is the central component of the openVALIDATION compiler. The AST is nothing else than the domain model of openVALIDATION. This domain model represents a logical structure of a validation rule.

![Schematic representation of an AST object and the rule contained inside it](../../.gitbook/assets/image%20%2822%29.png)

In the image you can see that e.g. a rule contains a condition and an error message. The condition has a left operand, a right operand and a comparison operator. This is a very simple example, which only demonstrates the schematic and logical structure of AST. Usually, the AST is much more complex. For example, a rule can contain many conditions, which are linked with a logical operator AND or OR. There are also nested conditions or condition groups. There are variables, which can contain another conditions, and so on.

The complete AST model consists of many individual classes which together form a logical hierarchy. [ASTModel](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTModel.java)  is the aggregate root of this central domain model.

![the package io.openvalidation.common.ast contains the AST model](../../.gitbook/assets/image%20%284%29.png)

The AST tree can easily be extended. For this purpose, each element must be derived at least from the class [ASTItem](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTItem.java). Depending on the position of the extension within the structure, a corresponding base class must be used.

{% hint style="info" %}
Parsing is primarily about extracting all relevant information from a continuous text and transferring it into an object structure, i.e. into the AST. The AST can then be processed further. The AST will be validated and if everything is OK, code will be generated from the AST.
{% endhint %}



### Validation

xxx

![](../../.gitbook/assets/image%20%2828%29.png)



### Generator

xxx

![](../../.gitbook/assets/image.png)



### Code

Am Ende des gesamten Kompiliervorgangs wird Programmcode erstellt. Abhängig von der Ausgabesprache kommt entsteht am Ende ein Code in Java, JavaScript, C\# oder in einer der verfügbaren Programmiersprachen. Der Code Selbst wird dabei in 3 Bereich unterteilt:

1. Implementierung

