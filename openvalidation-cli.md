---
description: Description of CLI usage
---

# Command Line Interface

## Prerequisites

To use **openValidation** you need Java version 8 or higher. The corresponding [JRE 1.8 ](https://www.oracle.com/technetwork/java/javase/downloads/index.html)can be downloaded here.

A command line interface \(CLI\) is a means of interacting with **openValidation.jar** where the user passes input to the program in the form of successive text lines \(command lines\).

The **openValidation.jar** is a simple Java Command Line interface which can be downloaded [here](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar).

 

## Options

The options specify the basic action that **openValidation.jar** should perform and provide additional information about how the command should work.

 The possible options are listed here:

| Parameters        | Shortcut | Description |
| :--- | :--- | :--- |
| --rule | `-r`  | Here you define the **validation rules** to be created with **openValidation** |
| --schema | `-s` | The **schema** defines the data structure of the data record |
| --culture | `-c` | This option defines the **natural language** in which the rules are defined. |
| --language | `-l` | This option defines the **programming language** in which the code is to be generated |
| --output | `-o` | The Output option defines a directory where the generated code files are stored. Without specifying the output parameter, the generated code is only displayed in the console |
| --single-file | `-f` | This option saves the output as a single file. This option depends on the path specified for storing the **output** |
| --params | `-p` | Input of certain parameters |
| --help | `-h` | Help for **openValidation** |
| --verbose | `-v` | Outputs the logs of **openValidation** |
| --al | `-a` | This option returns the list of **all available programming languages** |
| --no-banner | `-n` | This option determines the display of the banner |

### 

### Rule

The validation rule can be specified using the **Rule \(-r\)** option. This must have a certain format in order to be usable by the software.

This is also a **mandatory option**. Without the validation rule no meaningful code can be generated.

Every rule entered should have a certain structure to be readable by **openValidation**.

The structure is explicitly defined by the [grammar ](grammatik/)used by **openValidation**. The grammar divides the input into four possible elements, which have to be separated by a paragraph \(double empty line\).

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the message IS Hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the message IS Hello THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -c en
```
{% endtab %}
{% endtabs %}

The rule can be written as pure text. It is also possible to insert a **URL** or a **path** to a file with various validation rules. If there should be several rules in the same context it is recommended to save them in a **file** or **URL**. If there is a reference to a file it is recommended to format the file in **UTF-8**.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r C:/path_to_folder/rules.txt ^
     -s "{location : \"Berlin\"}" ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r /home/path_to_folder/rules.txt \
     -s "{location : \"Berlin\"}" \
     -c en
```
{% endtab %}
{% endtabs %}

### Schema

The **Schema \(-s\)** option can be used to define the schema. This allows **openValidation** to recognize which properties and types are required for the output.

This is also a **mandatory** option. Without entering the schema no code can be generated.

The schema is specified in **JSON** format. When referring to a file, it is recommended to format the file in **UTF-8**.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the type of the vehicle IS Bicycle THEN Bicycle couriers are desired" ^
     -s "{type : \"Bicycle\"}" ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the type of the vehicle IS Bicycle THEN Bicycle couriers are desired" \
     -s "{type : \"Bicycle\"}" \
     -c en
```
{% endtab %}
{% endtabs %}

To avoid errors when entering the schema, it is recommended to specify the path to the desired schema.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the type of the vehicle IS Bicycle THEN Bicycle couriers are desired" ^
     -s C:/path_to_folder/schema.json ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the type of the vehicle IS Bicycle THEN Bicycle couriers are desired" \
     -s /home/path_to_folder/schema.json \
     -c en
```
{% endtab %}
{% endtabs %}

### Culture

The **Culture \(-c\)** option can be used to specify the natural language of the validation rule. 

This option is not mandatory. If no natural language is specified, the language of the operating system user will be used.

It is important to note that the language of the validation rule also corresponds to the natural language defined by the **Culture** option.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt!" ^
     -s "{Nachricht : \"text\"}" ^
     -c de
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "ЕСЛИ cообщение привет ТОГДА привет мир!" \
     -s "{Сообщение : \"Текст\"}" \
     -c ru
```
{% endtab %}
{% endtabs %}

#### Languages

The following languages are currently supported:

| Language | Command |
| :--- | :--- |
| German | `-c de` |
| English | `-c en` |
| Russian | `-c ru` |

```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en
```

### Language

The **Language \(-l\)** option can be used to specify the programming language of the output. 

This option is not mandatory. If the programming language is not explicitly specified, **Java** is output by default.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -l javascript ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -l csharp \
     -c en
```
{% endtab %}
{% endtabs %}

#### Programming languages

The following programming languages are currently supported:

| Programming language | Command |
| :--- | :--- |
| Java | `-l java` |
| C\# | `-l csharp` |
| JavaScript | `-l javascript` |
| Python | `-l python` |

In addition, **all available programming languages** can be output using the **--al \(-a\)** option. This option can also be used without entering other options or **mandatory** options.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -a
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     --al
```
{% endtab %}
{% endtabs %}

### Output

The **Output \(-o\)** option can be used to save the output to a file or folder. If this option is not defined, the output will only be output to the console.

Specifies the path for files and folders to store the generated code.

In this path, the **rule** and the **framework** are generated in the corresponding programming language. Two files are always generated.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -o C:/path_to_folder ^
     -c en
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -o /home/path_to_folder \
     -c en
```
{% endtab %}
{% endtabs %}

### Single file

To reduce the output to one file, there is the option **Single file \(-f\)**. Enabling this option only creates a single file. This contains the rule and the corresponding framework.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -o C:/path_to_folder ^
     -c en ^
     -f
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -o /home/path_to_folder \
     -c en \
     -f
```
{% endtab %}
{% endtabs %}

### Verbose

With the help of the option **Verbose \(-v\)** additional output can be output. This helps to better understand the generated code and to understand the steps.

In addition, every step of the generation process can be analyzed in detail. This makes it easier to find errors and better verify possible incorrect entries.

In addition, all inputs are displayed as well as the step-by-step representation of the entire process.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en ^
     -v
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -c en \
     -v
```
{% endtab %}
{% endtabs %}

### Params

The **Params \(-p\)** allows you to customize the generated code. Several parameters are entered in the format name=value, separated by semicolons.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en ^
     -p model_type=io.openvalidation.Data;^
        generated_class_namespace=io.openvalidation.rules;^
        generated_class_name=MyRuleSet
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -c en \
     -p model_type=io.openvalidation.Data;\
        generated_class_namespace=io.openvalidation.rules;\
        generated_class_name=MyRuleSet
```
{% endtab %}
{% endtabs %}

**model\_type** \(default=empty\)   
is a complete\(package + classname\) name of the external model type. This specification is used to declare the model within the generated code.

**generated\_class\_namespace** \(default=io.openvalidation.rules\)   
is the name of the package of the generated classes. Both the framework and the implementation \(the actual rule set\) are assigned this package name.

**generated\_class\_name** \(default=HUMLValidator\)   
is the name of the generated class that contains the actual implementation of the rule set.

### No Banner

The **No-Banner \(-n\)** option can be used to remove the banner from the console.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hello THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en ^
     -n true
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "IF the Message EQUALS hallo THEN Hello World" \
     -s "{Message : \"Text\"}" \
     -c en \
     -n true
```
{% endtab %}
{% endtabs %}

### Help

If you use the **Help** option **\(-h\)**, help is displayed.

This option provides help for using **openValidation**.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -h
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -h
```
{% endtab %}
{% endtabs %}

