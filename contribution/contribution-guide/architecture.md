---
description: Description of the basic architecture
---

# Architecture

At this point the core functionality and the corresponding architecture of **openVALIDATION** is described. This description does not include the CLI or REST component. A natural language rule and the corresponding schema are expected as input parameters. Afterwards the processing starts, which can be separated into 5 subroutines \(preprocessor, schema converter, parser, validation, generator\). At the end of the compilation process program code is generated.

![simplified view of the entire compilation process](../../.gitbook/assets/image%20%2835%29.png)

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

These aliases will be resolved or normalized in the preprocessor.

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

openVALIDATION needs the schema to parse the natural language rule. All attributes of the schema are therefore loaded into the [DataSchema](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/data/DataSchema.java) component. To do this, the schema must be read and converted into the appropriate format. Since the schema itself can be specified in various formats, such as JSON Schema or JSON Object and later also in yaml or xsd, there is a [SchemaConverterFactory](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/converter/SchemaConverterFactory.java). It provides the corresponding implementation of a specific converter depending on the type of schema.

![the specified schema is loaded into the DataSchema component](../../.gitbook/assets/image%20%2812%29.png)

Each converter must implement the [ISchemaConverter](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/converter/ISchemaConverter.java) interface and the DataSchema **convert\(\)** method. The task of the converter is to run through the hierarchy of the schema and convert each attribute with all relevant meta informations, such as name, type, path/full name, and so on, into a flat structure of the DataSchema.

Here is an example. The following schema is given:

```javascript
{
    age:0,
    address : {
        city : ""
    }
}
```

After the conversion the following information is in the DataSchema \(pseudo YAML format\):

```javascript
_properties:
    DataProperty:
        name : "age"
        path : ""
        fullName : "age"
        type : "Decimal"
    DataProperty:
        name : "address"
        path : ""
        fullName : "address"
        type : "Object"
    DataProperty:
        name : "city"
        path : "address"
        fullName : "address.city"
        type : "String"                

        
```

Thanks to this metadata, all relevant information can be extracted from a natural language rule. Among other things, this construct allows recursive name resolution of schema attributes. For example, you could directly use the attribute city without specifying the full name address.city like in a formal programming language. If city occurs more than once in schema, it will be validated later and the user will get a compiler message that he has to use the full name because of ambiguity.

**DataSchema**

The DataSchema component stores not only the schema information but also the [variables](../../grammatik/variablen.md) and [semantic operators](../../grammatik/domainspezifische-operatoren.md). However, this information is added at a later time after the first parsing routine.

By keeping the type information available during parsing, especially the operands of a condition can be determined and extracted. For example, if you know that the left operand is an attribute from the schema of type decimal and the right operand simply contains text, you can try to extract a numeric value from this text. If no numeric value is found, you can then throw a relatively unique compiler message.



### Preprocessor

Before a natural language rule can be parsed, the corresponding text must be prepared. For example, the keywords or the paragraphs must be normalized. Includes must be resolved or it must be checked if there are collisions with different keywords and so on.

These are many small routines that prepare the original text. This processing logic has also been modularized. So there are many small preprocessors that perform the individual tasks one after the other.

![The internal design of the preprocessor](../../.gitbook/assets/image%20%2810%29.png)

Every single step of the whole preprocessor routine is implemented in a separate module. Each of these modules must be derived from the abstract base class [PreProcessorStepBase](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/PreProcessorStepBase.java). Afterwards the method **String process\(String rule\)** must be overwritten and provided with the respective logic. Currently, the following modules can be found in the package [io.openvalidation.core.preprocessing.steps](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps):

[PreProcessorAliasResolutionStep.java](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps/PreProcessorAliasResolutionStep.java)

[PreProcessorIncludeResolutionStep.java](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps/PreProcessorIncludeResolutionStep.java)

[PreProcessorKeywordCollisionStep.java](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps/PreProcessorKeywordCollisionStep.java)

[PreProcessorLastParagraphCleanup.java](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps/PreProcessorLastParagraphCleanup.java)

[PreProcessorVariableNamesStep.java](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/java/io/openvalidation/core/preprocessing/steps/PreProcessorVariableNamesStep.java)

![](../../.gitbook/assets/image%20%2818%29.png)

{% hint style="info" %}
Further modules can be implemented here without problems. It is very important to keep the order in which the individual modules are executed.
{% endhint %}

#### Resolving the keywords

The resolution of keywords is one of the most important tasks of the preprocessor. It involves converting the large number of aliases into a normalized form. The following parsing process only knows the normalized keywords and not their aliases.

And this is how it looks like. We have the following rule:  


```javascript
applicant's age should not be less than 18 years
```

The preprocessor makes of it:

```javascript
the applicant's age 
ʬconstraintʬmustnotʬshould_20_not be ʬoperatorʬless_20_thanʬless 
than 18 years

```

**should not** became **ʬconstraintʬmustnotʬshould\_20\_not** where the last part ...**should\_20\_not** is dynamic. At this point, the original text is preserved so that it is available after the parsing process, for example, for compiler messages or for other purposes. The front part **ʬconstraintʬmustnotʬ** is normalized. This is the part the parser knows. 

{% hint style="warning" %}
The resolution of the aliases depends on the Culture Code used. When using e.g. **de**, only aliases from the German resource [aliases\_en.properties](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-core/src/main/resources/aliases_de.properties) are used.
{% endhint %}



### Parser

Parsing is probably the most important and complex processing step in openVALIDATION. One of the reasons for the particular complexity is the flexibility of the grammar or Natural Language. In formal programming languages but also in most DSL's and especially in the strongly typed object-oriented programming languages every word, every character has a very precise meaning. The input must be very exact otherwise there is an immediate compiler error.

While designing the grammar of openVALIDATION it was very important to us to avoid such restrictions as they make the understanding and usage for normal users \(**non-developers**\) difficult. Therefore openVALIDATION allows many variants of how to express a rule in a natural way without bothering the user with a compiler message at every little deviation.

```javascript
if user's age is less than 18 years then underage persons are not admitted
```

or

```javascript
the applicant's age must not be less than 18 years
```

Both rules are translated by the compiler into the following code. Here in pseudo code:

```javascript
if (age < 18)
    throw Error("underage persons..." or in the 2nd case "the applicant's..")
```

The compiler first tries to accept the input as it suits the user best, then extracts the relevant part without forcing the user to follow a certain notation. This makes the learning curve for Newbies extremely steep. You only need to get a rough idea of the structure of a validation rule, without having to pay attention to every character or every special keyword.

openVALIDATION requires relatively little abstraction ability on the part of the user. The compiler takes over this task itself and thus relieves the user at this point. This is the core of the philosophy behind openVALIDATION:

> "Instead of forcing humans to understand the complex inner workings of machines, we should construct machines in a way, so they better understand us humans"

To make this convenience available to the user, the complexity must be shifted into the processing logic of the compiler. Especially the parser does most of the work at this point. It takes care that a complex hierarchical Abstract Syntax Tree \(AST\) is created from a single string.

![the parser takes a normalized rule and creates an AST object](../../.gitbook/assets/image%20%2848%29.png)

To make this possible the parser itself consists of 3 components:

1. ANTLR Grammar\(Lexer, Parser, Generator...\)
2. Parse Tree Transformer
3. Post Processors

Each of these sub-components has a specific task and therefore involves a certain complexity. The entire logic for parsing the validation rules is contained in the package [io.openvalidation.antlr](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr).



#### ANTLR Parser

ANTLR Parser takes care of the initial step of parsing. At this point, a generic AST is created from a text. This is called **ParseTree** in ANTLR. At this point, the text is transferred to a temporary object tree.

The core of the ANTL parsing routine is the corresponding ANTL grammar of openVALIDATION. It is located in the file [main.g4](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/resources/main.g4) in the Resources folder of the [openvalidation-antlr](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-antlr) module. Here is a small excerpt of it:

```javascript
grammar main;

main                     : PARAGRAPH? (rule_definition|rule_constrained|variable|semantic_operator|comment|unknown)
                            (PARAGRAPH (rule_definition|rule_constrained|variable|semantic_operator|comment|unknown))*
                             PARAGRAPH? unknown? EOF;


comment                  :  STRING? COMMENT unknown?;
variable                 :  (lambda|expression)? AS name?;
semantic_operator        :  unknown? OPERATOR_COMP? AS_OPERATOR name?;
rule_definition          :  IF? expression? THEN action?
                         |  IF expression? THEN? action?;

...
```

In this grammar the so-called lexer tokens and the parser rules are defined. While lexer tokens represent the individual keywords, parser rules represent complete signatures of words or the tokens.

Here is an example for the signature of a comment:

```javascript
comment                  :  STRING? COMMENT unknown?;


unknown                  : (STRING | LPAREN | RPAREN | WITH_ERROR | COMBINATOR | OPERATOR_ARITH | WITH | OF | CONSTRAINT | IF | THEN | OPERATOR_COMP | AS | COMMENT | FUNCTION | FROM | ORDERED_BY)+;
COMMENT                  :  'ʬcommentʬ'[a-zA-Z0-9_]+;
STRING                   :  ~('ʬ')+;
```

A comment consists of an optional lexer token called **STRING** followed by the LEXER token **Comment** and concludes with an optional parser rule **unknown**. As soon as the string **ʬcommentʬxxx** appears somewhere in a paragraph, then this parser rule automatically takes effect.

Using this grammar, Java code is generated at design time using **antlr4-maven-plugin**'s Java code. This code is then placed in the target directory **target/generated-sources/java/io/openvalidation/antlr/generated** and is automatically integrated into the main project.

This code is then aggregated in the class [ANTLRExecutor](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/ANTLRExecutor.java). There the [MainASTBuildListener](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/MainASTBuildListener.java) is also included, which is the entry point for the following transformation logic.



#### Parse Tree Transformer

The package [io.openvalidation.antlr.transformation](https://github.com/openvalidation/openvalidation/tree/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation) contains the transformation logic of the parser. This logic mainly takes care that the generic parse tree generated by the ANTLR parser at runtime is transformed into the domain-specific AST model. Like many openVALIDATION components, the transformation logic is quite modular. Each single module takes care of a certain logical area. For example, it is the task of [PTCommentTransformer](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation/parsetree/PTCommentTransformer.java) to transform the corresponding part of the parse tree, namely the **mainParser.CommentContext** into the [ASTComment](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTComment.java). The structure of these transformers is hierarchical and follows the logical hierarchy of the parse tree, which in turn results from the grammar \(**main.g4**\).

Each individual transformer is derived from the abstract base class [TransformerBase](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation/TransformerBase.java) and must implement the method **transform\(\)**. There the actual transformation between the two data models takes place. The base method **ASTItem createASTItem\(ParseTree\)** can be used to transform the child elements. By calling this method with the corresponding Parse Tree, the transformation runs hierarchically from top to bottom. The [TransformerFactory](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation/TransformerFactory.java) is used to load the corresponding Transformer Module depending on the type of the **Parse Tree**.

After each transformation the corresponding post processor is called. 

#### 

#### Post Processors

In the transformation step, the AST elements and the entire tree are created only very roughly. It requires many further processing steps to transform the AST into its final form. The post processors take over this task. These are also modular and follow the AST hierarchy. Depending on the type, the individual modules are called only after a certain transformation step has been completed. The naming convention of the post processors ensures that the execution levels can already be recognized by their names. For example, a [PostConditionImplicitBoolOperand](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation/postprocessing/PostConditionImplicitBoolOperand.java) is called directly after the transformation of a condition, while a [PostModelNumbersResolver](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-antlr/src/main/java/io/openvalidation/antlr/transformation/postprocessing/PostModelNumbersResolver.java) is called only after the transformation of the AST model, i.e. at the very end of the entire transformation.

What exactly happens in the post processors? Let's take a look at the already implemented **PostConditionImplicitBoolOperand**:

```java
public class PostConditionImplicitBoolOperand extends PostProcessorSelfBase<ASTCondition> {

  @Override
  protected Predicate<ASTCondition> getFilter() {
    return c ->
        (c != null
            && !c.hasRightOperand()
            && c.hasLeftOperand()
            && c.hasEqualityComparer()
            && c.getLeftOperand().isPropertyOrVariable()
            && c.getLeftOperand().isBoolean());
  }

  @Override
  protected void processItem(ASTCondition condition) {

    if (!condition.hasRightOperand()) {

      ASTOperandStatic staticBool = new ASTOperandStatic("true");
      staticBool.setDataType(DataPropertyType.Boolean);
      staticBool.setSource("");
      condition.setRightOperand(staticBool);
    }
  }
}
```

The task of this post processor is to complete an incompletely transformed AST condition. A condition always consists of a left and a right operand. In between there is the comparison operator. The method **getFilter\(\)** searches for certain conditions, namely for those that have only one boolean operand and a comparison operator that is either Equals or Not Equals. Only if the condition of the method getFilter\(\) is fulfilled, the method **processItem\(\)** is called. There the already existing but still incomplete **ASTCondition** will be completed with an operand. Since it is an implicit boolean comparison, only a static **true** is added as the right operand.

This makes the following rules possible:

```javascript
the contract must be signed
```

and here is the corresponding scheme:

```javascript
{signe:true}
```

These and many other post processors form a very flexible framework, which makes it possible to manipulate the AST at specific points. 



![different states of a rule during the parsing process](../../.gitbook/assets/image%20%2816%29.png)



### Abstract Syntax Tree \(AST\)

The Abstract Syntax Tree is the central component of the openVALIDATION compiler. The AST is nothing else than the domain model of openVALIDATION. This domain model represents a logical structure of a validation rule.

![Schematic representation of an AST object and the rule contained inside it](../../.gitbook/assets/image%20%2825%29.png)

In the image you can see that e.g. a rule contains a condition and an error message. The condition has a left operand, a right operand and a comparison operator. This is a very simple example, which only demonstrates the schematic and logical structure of AST. Usually, the AST is much more complex. For example, a rule can contain many conditions, which are linked with a logical operator AND or OR. There are also nested conditions or condition groups. There are variables, which can contain another conditions, and so on.

The complete AST model consists of many individual classes which together form a logical hierarchy. [ASTModel](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTModel.java)  is the aggregate root of this central domain model.

![the package io.openvalidation.common.ast contains the AST model](../../.gitbook/assets/image%20%2854%29.png)

The AST tree can easily be extended. For this purpose, each element must be derived at least from the class [ASTItem](https://github.com/openvalidation/openvalidation/blob/master/openvalidation-common/src/main/java/io/openvalidation/common/ast/ASTItem.java). Depending on the position of the extension within the structure, a corresponding base class must be used.

{% hint style="info" %}
Parsing is primarily about extracting all relevant information from a continuous text and transferring it into an object structure, i.e. into the AST. The AST can then be processed further. The AST will be validated and if everything is OK, code will be generated from the AST.
{% endhint %}



### Validation

After the AST has been successfully generated, it has to be validated before program code is generated in the next step with the help of the generator. This validation step ensures that the AST tree is consistent and valid.

![the processing step Validation checks the AST and generates corresponding compiler messages](../../.gitbook/assets/image%20%2832%29.png)



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

![the package io.openvalidation.core.validation contains the corresponding validators](../../.gitbook/assets/image%20%2858%29.png)

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

![](../../.gitbook/assets/image%20%2857%29.png)

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

