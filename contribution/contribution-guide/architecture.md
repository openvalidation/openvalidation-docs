---
description: Description of the base architecture
---

# Architecture

The basic functionality of the compiler is relatively simple. A natural language rule and the corresponding schema are expected as input parameters. Afterwards the processing starts, which can be separated into 3 subroutines \(preprocessor, parser, generator\). At the end of the compilation process program code is generated.

![simplified view of the compilation process](../../.gitbook/assets/image%20%281%29.png)

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

xxx



### Preprocessor

xxx



### Parser

xxx



### AST

xxx



### Generator

xxx



### Code

xxx

