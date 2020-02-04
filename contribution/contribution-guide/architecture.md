---
description: Description of the basic architecture
---

# Architecture

At this point the core functionality and the corresponding architecture of **openVALIDATION** is described. This description does not include the CLI or REST component. A natural language rule and the corresponding schema are expected as input parameters. Afterwards the processing starts, which can be separated into 5 subroutines \(preprocessor, schema converter, parser, validation, generator\). At the end of the compilation process program code is generated.

![simplified view of the entire compilation process](../../.gitbook/assets/image%20%2831%29.png)

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

![the parser takes a normalized rule and creates an AST object](../../.gitbook/assets/image%20%2843%29.png)



### Abstract Syntax Tree \(AST\)

The Abstract Syntax Tree is the central component of the openVALIDATION compiler. The AST is nothing else than the domain model of openVALIDATION. This domain model represents a logical structure of a validation rule.

![Schematic representation of an AST object and the rule contained inside it](../../.gitbook/assets/image%20%2822%29.png)

In the image you can see that e.g. a rule contains a condition and an error message. The condition has a left operand, a right operand and a comparison operator. This is a very simple example, which only demonstrates the schematic and logical structure of AST. Usually, the AST is much more complex. For example, a rule can contain many conditions, which are linked with a logical operator AND or OR. There are also nested conditions or condition groups. There are variables, which can contain another conditions, and so on.

The complete AST model consists of many individual classes which together form a logical hierarchy. [ASTModel](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTModel.java)  is the aggregate root of this central domain model.

![the package io.openvalidation.common.ast contains the AST model](../../.gitbook/assets/image%20%2848%29.png)

The AST tree can easily be extended. For this purpose, each element must be derived at least from the class [ASTItem](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTItem.java). Depending on the position of the extension within the structure, a corresponding base class must be used.

{% hint style="info" %}
Parsing is primarily about extracting all relevant information from a continuous text and transferring it into an object structure, i.e. into the AST. The AST can then be processed further. The AST will be validated and if everything is OK, code will be generated from the AST.
{% endhint %}



### Validation

Nachdem der AST erfolgreich erzeugt wurde, muss dieser noch validiert werden bevor daraus im nächsten Schritt Programmcode mit Hilfe des Generators generiert wird. Dieser Validierungsschritt sorgt dafür, dass der AST Baum konsistent und valide ist. 

![der Verarbeitungsschritt Validierung pr&#xFC;ft den AST und erzeugt logische Compilermeldungen](../../.gitbook/assets/image%20%2828%29.png)



Hier ist ein Beispiel für eine mögliche Inkonsistenz. Das schema sieht folgendermaßen aus:

```javascript
{
    name:""
}
```

und diese Regel möchten wir generieren:

```javascript
applicant's age should not be less than 18 years
```

In dieser Regeln wird das Attribut age verwendet, allerdings enthält das Schema lediglich nur das Attribut name. Das Attribut age kann bzw. darf nicht gefunden werden. Der openVALIDATION Compiler darf in diesem Fall keinen Programmcode generieren. An dieser Stelle muss eine entsprechende Fehlermeldung vom Compiler geworfen werden: wie z.B. "Die Regel muss mindesten ein Attribut aus dem Schema beinhalten." Damit so eine Fehlermeldung geworfen werden kann, muss der AST entsprechend geprüft werden. 

Solche und viele weitere Prüfungen finden in dem Verarbeitungsschritt "Validation" statt. Diese Prüfmechanismen sind modular implementiert und befinden sich im openvalidation-core Modul.

![das Package io.openvalidation.core.validation enth&#xE4;lt die entsprechenden Validatoren](../../.gitbook/assets/image%20%2851%29.png)

Die ValidatorFactory erzeugt für jedes einzelne Element des AST's abhängig von dessen Typ eine neue Instanz des entsprechenden Validators. Somit kümmert sich jeder Validator um einen bestimmten Bereich des AST's. 

Jeder einzelne Validator wird dabei von der Basisklasse ValidatorBase abgeleitet und muss dabei die geerbte Methode void validate\(\) überschreiben. Sollte eine inkonsistenz gefunden werden, wird sofort eine neue Exception vom Typ ASTValidationException geworfen. Das sorgt dafür, dass keine unnötigen Folgefehler erfasst werden.   

{% hint style="warning" %}
Es ist eine der größten Herausforderungen im Bau eines Compilers saubere und aussagekräftige Compilermeldung zu erzeugen. Diese Fehlermeldungen sollen dem Anwender helfen, seine Eingabe zu korrigieren. Es geht unter anderem darum, nicht zu viel und nicht zu wenig Fehlerinformationen zurückzugeben. Oft ist es so, dass der eigentliche Fehler 10 Verarbeitungsschritte zurückliegt und lediglich seine Auswirkungen gefunden wurde. 

Das ExceptionHandling bzw. das generieren aussagekräftiger Compilerfehler in openVALIDATION befindet sich noch in einem noch sehr frühem Stadium. Es ist sehr wahrscheinlich, dass sich die Architektur an dieser Stelle nochmal ändern wird. 

Verbesserungsvorschläge und weitere Diskussionen zu diesem Thema sind sind ausdrücklich erwünscht!
{% endhint %}





### Generator

xxx

![](../../.gitbook/assets/image.png)



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

