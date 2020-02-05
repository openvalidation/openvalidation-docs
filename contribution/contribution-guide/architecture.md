---
description: Description of the basic architecture
---

# Architecture

At this point the core functionality and the corresponding architecture of **openVALIDATION** is described. This description does not include the CLI or REST component. A natural language rule and the corresponding schema are expected as input parameters. Afterwards the processing starts, which can be separated into 5 subroutines \(preprocessor, schema converter, parser, validation, generator\). At the end of the compilation process program code is generated.

![simplified view of the entire compilation process](../../.gitbook/assets/image%20%2832%29.png)

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

![the specified schema is loaded into the DataSchema component](../../.gitbook/assets/image%20%2812%29.png)



### Preprocessor

xxx

  


![The internal design of the preprocessor](../../.gitbook/assets/image%20%2810%29.png)

xx



### Parser

xxx

![the parser takes a normalized rule and creates an AST object](../../.gitbook/assets/image%20%2844%29.png)



### Abstract Syntax Tree \(AST\)

The Abstract Syntax Tree is the central component of the openVALIDATION compiler. The AST is nothing else than the domain model of openVALIDATION. This domain model represents a logical structure of a validation rule.

![Schematic representation of an AST object and the rule contained inside it](../../.gitbook/assets/image%20%2823%29.png)

In the image you can see that e.g. a rule contains a condition and an error message. The condition has a left operand, a right operand and a comparison operator. This is a very simple example, which only demonstrates the schematic and logical structure of AST. Usually, the AST is much more complex. For example, a rule can contain many conditions, which are linked with a logical operator AND or OR. There are also nested conditions or condition groups. There are variables, which can contain another conditions, and so on.

The complete AST model consists of many individual classes which together form a logical hierarchy. [ASTModel](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTModel.java)  is the aggregate root of this central domain model.

![the package io.openvalidation.common.ast contains the AST model](../../.gitbook/assets/image%20%2850%29.png)

The AST tree can easily be extended. For this purpose, each element must be derived at least from the class [ASTItem](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTItem.java). Depending on the position of the extension within the structure, a corresponding base class must be used.

{% hint style="info" %}
Parsing is primarily about extracting all relevant information from a continuous text and transferring it into an object structure, i.e. into the AST. The AST can then be processed further. The AST will be validated and if everything is OK, code will be generated from the AST.
{% endhint %}



### Validation

After the AST has been successfully generated, it has to be validated before program code is generated in the next step with the help of the generator. This validation step ensures that the AST tree is consistent and valid.

![the processing step Validation checks the AST and generates corresponding compiler messages](../../.gitbook/assets/image%20%2829%29.png)



Here is an example of a possible inconsistency. The schema looks like this:

```javascript
{
    name : ""
}
```

and here is the rule:

```javascript
applicant's age should not be less than 18 years
```

The **age** attribute is used in these rules, but the schema only contains the attribute **name**. The **age** attribute cannot or must not be found. In this case openVALIDATION Compiler must not generate any program code. At this point, the compiler must throw a corresponding error message: e.g. **"The rule must contain at least one attribute from the schema"** or something like that. In order for such an error message to be thrown, the AST must be checked accordingly.

Such and many other checks are performed in the processing step "Validation". These check mechanisms are implemented in a modular way and are located in the [openvalidation-core](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-core) module.

![the package io.openvalidation.core.validation contains the corresponding validators](../../.gitbook/assets/image%20%2854%29.png)

The ValidatorFactory creates a new instance of the corresponding validator for each individual element of the AST depending on its type. Thus, each validator takes care of a certain area of the AST.

Each single validator is derived from the base class **ValidatorBase** and must overwrite the inherited method **void validate\(\)**. If an inconsistency is found, a new exception of type **ASTValidationException** is thrown immediately. This ensures that no unnecessary subsequent errors are caught.

{% hint style="warning" %}
It is one of the biggest challenges in developing a compiler to produce clean and meaningful compiler messages. These error messages should help the user to correct his input. Among other things, it is important not to return too much and not too few error information. It is often the case that the actual error occurred 10 processing steps back and only its site effects were found.

The exception handling or the generation of meaningful compiler errors in openVALIDATION is still in a very early stage. It is very likely that the architecture will change again at this point.

Suggestions for improvement and further discussions on this topic are explicitly welcome!
{% endhint %}





### Generator

This is the final processing step. At this point program code is generated from the AST tree. To be more precise, OpenValidationResult is generated, which contains compiler error messages as well as various metadata in addition to the actual code.

The core of the code generation are the generator templates and the framework [Handlebars](https://handlebarsjs.com/)  or their [java implementation](https://github.com/jknack/handlebars.java).

![Program code is generated from the AST tree using Handlebars Templates](../../.gitbook/assets/image.png)

openVALIDATION is a multilingual cross compiler. It means that both input and output can be done in different languages. Therefore there are several generator templates for different programming languages. Here is the list of the currently supported languages, but not all of them are implemented yet:

* Java
* JavaScript/node
* C\#
* Python
* Rust

More are to follow.

Everything that is necessary to generate is located in the module [openvalidation-generation](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-generation). The corresponding generator templates are located in the Resources folder:

![](../../.gitbook/assets/image%20%2853%29.png)

Each supported programming language has its own folder with the corresponding name. Generator templates for Javascript are located in the folder Javascript, for Java in Java, and so on. Cross-language templates are located in the folder common.

The templates themselves are named by the type of AST element and are used accordingly during generation. There is also a fallback mechanism that checks whether a template with the corresponding name exists in the language-specific folder. If not, the template is loaded from the Common Folder. This fallback mechanism enables the consistent reuse of templates for different output languages. Thus, you need much less language-specific generator templates and can implement new programming languages with much less effort.

This fallback was realized with an own helper called [tmpl](https://github.com/openvalidation/openvalidation/blob/b9dd7dfa627019063b3d3a8263d1ede8114ba28f/openvalidation-generation/src/main/java/io/openvalidation/generation/CodeGenerator.java#L110).

Besides the actual rules, the generator creates its own specific framework and another code artifact called ValidatorFactory. The framework and the factory exist for each output language. They later facilitate the integration of the generated code into other systems.

Above all, the HUMLFramework or the corresponding framework.hbs is full of specific logic that requires special quality assurance. In order to be able to test it properly, the framework.hbs templates have been outsourced to separate projects within our own [openvalidation-framework-tests](https://github.com/openvalidation/openvalidation-framework-tests) repositories. There the respective implementations can be tested in their own technology stacks, each with its own unit tests. Afterwards one has to copy the corresponding code from the respective file HUMLFramework.xxx into the corresponding directory within the generator as framework.hbs. This is currently a manual step, which is required at this point. In the future, this should also be automated or simplified.



### Code

At the end of the entire compilation process, depending on the output language, program code is generated in Java, JavaScript, C\# or in one of the supported programming languages. The code itself is divided into 3 categories:

* Implementation
* Framework
* ValidatorFactory

#### **Implementation**

The implementation code contains the actual program logic of the respective Rule Set. It is the part of the code that has been translated from a natural language into the respective programming language. And this is what the code looks like:

```javascript
var HUMLValidator = function() {
    var huml = new HUMLFramework();

    //rule: applicant's age should not be less than 18 years
    huml.appendRule("",
           ["age"],
           "applicant&#x27;s age should not be less than 18 years",
           function(model) { return huml.LESS_THAN(model.age, 18.0);},
           false
        );

    this.validate = function(model){
        return huml.validate(model);
    }
}
```

The example above shows the generated JavaScript code. The code consists of a basic structure which, in the case of JavaScript, is mapped by a function called **HUMLValidator**\(default name\) to the actual rule set defined by **huml.appendRule** and by the function **validate**.

#### The Framework

The implementation uses its own framework called HUMLFramework. This framework contains all the necessary basic functions for comparing different values. There are 2 important reasons why the HUMLFramework is necessary:

1. The generated code does not have any dependencies to the 3'rd party libraries. The generated code includes everything necessary. This makes the integration of the generated code much easier.
2. The framework serves as a kind of normalization layer for cross-language code generation. The structure of the rules behind **huml.appendRule** looks similar in different programming languages. This allows us to reuse code generation templates when generating code in different programming languages - see [Generator](architecture.md#generator).

#### ValidatorFactory

It is possible to generate several rule sets simultaneously. In this case there are several validators, each with its own rules. With the help of the **ValidatorFactory** you can access the specific rule set using the unique **ID**:

```javascript
var openValidatorFactory = function(){
    var _validators = {
            Validator1 : new Validator1(),
            Validator2 : new Validator2(),
            Validator3 : new Validator3(),            
    };

    this.create = function(validatorID){
        if (_validators != null){
            return _validators[validatorID];
        }

        return null;
    }
}
```

And here is how to use the ValidatorFactory:

```javascript
//usage:
var factory = openValidatorFactory();
validator = factory.create("Validator1");
validator.validate({age:18})
```



{% hint style="info" %}
Code files can be generated automatically by setting the appropriate parameters. Furthermore, it is possible to create the entire code in one file each. Depending on the programming language, e.g. Java, inner classes will be generated.
{% endhint %}

