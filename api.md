---
description: Verwendung der Java API
---

# API

openVALIDATION wird hauptsächlich als CLI in der Konsole verwendet. Zudem besteht die Möglichkeit, zur besseren Integration in Java Projekten, direkt auf die API von openVALIDATION zu zuzugreifen. Dazu stellt openVALIDATION ein Maven Package namens **openvalidation-core** bereit:

```markup
<dependency>
    <groupId>org.bag.openvalidation</groupId>
    <artifactId>openvalidation-core</artifactId>
    <version>1.0-SNAPSHOT</version>
    <scope>compile</scope>
</dependency>
```

Als Erstes wird eine Instanz der Klasse `org.bag.opernvalidation.core.OpenValidation` benötigt.  
Diese wird folgendermaßen instantiiert:

```java
OpenValidation ov = OpenValidation.createDefault();
```

Anschließend können Parameter initialisiert werden:

```java
ov.setLanguage(Language.Node)
ov.setLocale("en");
ov.setSchema("{Alter:0}");
ov.setRule("das Alter des Bewerbers MUSS MINDESTENS 18 sein");
```

Jetzt kann der Code generiert werden:

```java
OpenValidationResult result = ov.generateCode();
```

Das Objekt **result** vom Typ `org.bag.openvalidation.common.model.OpenValidationResult`enthält alle Informationen und Ergebnisse des Generierungsvorgangs. Unter anderem die Information, ob Fehler aufgetreten sind.

```java
if ( ov.hasErrors() ) {
    Sytem.out.print(ov.toErrorString(true)) //verbose: true
}
```

Wenn es keine Fehler gab, dann kann man mit den folgenden Aufrufen auf die Code-Ergebnisse zugreifen:

```javascript
//get implemented rules
List<CodeGenerationResult> implResults = ov.getImplementationResults(); 

//get framework part
CodeGenerationResult framework = ov.getFrameworkResult()
```

Der generierte Code besteht immer aus 2 Komponenten. Aus den implementierten Regeln und dem eigenen Framework. Das Framework sorgt dafür, dass der generierte Code keine externen Abhängigkeiten hat. 

So erfolgt der Zugriff auf den Code:

```java
String rulesCode = implResults.get(0).getCode();
String frameworkCode = framework.getCode();
```

Und so sehen am Ende die generierten Regeln in Node.js bzw. JavaScript aus:

```javascript
var huml = require('./HUMLFramework');

var HUMLValidator = function() {

    huml.appendRule("",
           ["Alter"],
           "das Alter des Bewerbers MUSS MINDESTENS 18 sein",
           function(model) { return  huml.LESS_THAN(model.Alter, 18.0);},
           false
    );

    this.validate = function(model){
        return huml.validate(model);
    }
}
```

