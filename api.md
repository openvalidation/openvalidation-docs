---
description: Using the Java API
---

# API

openVALIDATION is mainly used as CLI in the console. However, it is possible to directly access the API of openVALIDATION for better integration in Java projects. For this purpose openVALIDATION provides a Maven package called **openvalidation-core**:

```markup
<dependency>
    <groupId>org.bag.openvalidation</groupId>
    <artifactId>openvalidation-core</artifactId>
    <version>1.0-SNAPSHOT</version>
    <scope>compile</scope>
</dependency>
```

First you need an instance of the class `org.bag.opernvalidation.core.OpenValidation`. This is instantiated as follows:

```java
OpenValidation ov = OpenValidation.createDefault();
```

Parameters can now be initialized:

```java
ov.setLanguage(Language.Node)
ov.setLocale("en");
ov.setSchema("{age:0}");
ov.setRule("the age of the applicant MUST be AT LEAST 18");
```

Now the code can be generated:

```java
OpenValidationResult result = ov.generateCode();
```

The object **result** of the type `org.bag.openvalidation.common.model.OpenValidationResult`contains all information and results of the generation process. Among other things, the information whether errors occurred.

```java
if ( ov.hasErrors() ) {
    Sytem.out.print(ov.toErrorString(true)) //verbose: true
}
```

If there were no errors, then you can access the code results with the following calls:

```javascript
//get implemented rules
List<CodeGenerationResult> implResults = ov.getImplementationResults(); 

//get framework part
CodeGenerationResult framework = ov.getFrameworkResult()
```

The generated code always consists of two components. From the implemented rules and your own framework. The framework ensures that the generated code has no external dependencies.

This is how the code is accessed:

```java
String rulesCode = implResults.get(0).getCode();
String frameworkCode = framework.getCode();
```

This is how the generated rules look like in Node.js or JavaScript:

```javascript
var huml = require('./HUMLFramework');

var HUMLValidator = function() {

    huml.appendRule("",
           ["age"],
           "the age of the applicant MUST be AT LEAST 18",
           function(model) { return  huml.LESS_THAN(model.age, 18.0);},
           false
    );

    this.validate = function(model){
        return huml.validate(model);
    }
}
```

