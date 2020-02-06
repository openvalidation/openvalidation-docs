# Ready to go in 3 min.

### Prerequisites

You need Java version 8 or higher. Here you can download the corresponding [JRE 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).

### 1. Installation

You need **openvalidation.jar** to generate code. **openvalidation.jar** is a simple Java command line interface. You can download the **openvalidation.jar** here:

[![](.gitbook/assets/button1%20%285%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar) 

{% tabs %}
{% tab title="Windows" %}
Best you create a new directory under your personal Temp folder and download the **openvalidation.jar** into this folder:

```bash
cd %temp%
mkdir openvalidation
cd openvalidation
curl http://download.openvalidation.io/openvalidation.jar
```
{% endtab %}

{% tab title="Unix" %}
Best you create a new directory under your personal Temp folder and download the **openvalidation.jar** into this folder:

```bash
cd /tmp/
mkdir openvalidation
cd openvalidation
curl http://central.maven.org/maven2/openvalidation.jar
```
{% endtab %}
{% endtabs %}

### 2. Create a rule

Ok, now you are ready to start and can program your first validation rule in openVALIDATION. Create a file called _rules.ov_ and add the following content:

{% code title="rules.ov" %}
```coffeescript
  IF the message IS Hello
THEN hello openVALIDATION!
```
{% endcode %}

_This rule is formatted so that it best illustrates the keywords and syntax of the openVALIDATION language. You could also just write `If the message is Hello then hello openVALIDATION!` and it would work just the same._

In addition to the actual rule, you also need a schema. The schema defines the data structure of the data set to be validated. You can use a simple JSON object from which a JSON schema is automatically derived. Our schema consists only of a text field called "Message".

{% tabs %}
{% tab title="YAML" %}
```yaml
Message : Text
```
{% endtab %}

{% tab title="JSON" %}
```scheme
{
    Message : "Text"
}
```
{% endtab %}
{% endtabs %}

### 3. Generate code

Since our validation rule and the corresponding schema are already fixed, we can now generate the corresponding code. 

```java

java -jar openvalidation.jar /
     -r rules.ov /
     -s "{Message : \"Text\"}" /
     -l javascript /
     -o myRules.js /
     -c en /
     -f

```

| Parameter        | Description |
| :--- | :--- |
| -r | The **Rule** parameter is used to pass the actual validation rule either directly or as a file reference. In our case, we pass the file _rules.ov_ |
| -s | The schema specifies the data structure for openVALIDATION. |
| -l | The programming language to be translated  |
| -c | The culture code of natural language \(**de** for German/ **en** for English\) |
| -o | Output Parameter defines the code file. Alternatively you can specify a directory where the code files will be stored after the generation process. Without specifying the output parameter, the generated code will only be output in the console |
| -f | This parameter \(--single-file\) ensures that the generated code is generated in a file. Normally there are two files: the base framework and the implementation of the rules |

Congratulations! You have just created your first validation rule in JavaScript. This can be found in the file _myRules.js_. This is the result:

{% code title="" %}
```javascript
...

huml.appendRule("",
     ["Message"],
     "Hello openVALIDATION!",
     function(model) { return  huml.EQUALS(model.Message, "Hello");},
     false
);

...
```
{% endcode %}



### 4. Rules integration

Now we'd like to see if our rule works. We create a file called _test.html_ and add the following code:

{% code title="test.html" %}
```markup
<html>
  <body>

    <input id="message" />
    <div id="error"></div>

    <script src="myRules.js"></script>
    <script type="text/javascript">

      window.addEventListener('load', (event) => {

        var validator = new HUMLValidator();

        document.getElementById('message').onkeyup = function(e){
            var result = validator.validate({Message:e.target.value});

            if (result.hasErrors)
              document.getElementById('error').textContent = result.errors[0].error;
            else
              document.getElementById('error').textContent = "";
        };
      });

    </script>
  </body>
</html>
```
{% endcode %}

We call the **test.html** in the browser and type "_Hello_" into the input field:

![](.gitbook/assets/image%20%2841%29.png)

As expected, when entering the value "_Hello_", our error message appears.



### 5. No-Code Development

You can now extend the rules by filling the file _rules.ov_ with additional rules and then doing the generation process again using openVALIDATION CLI. Just follow steps 2 to 4.

The nice thing about it is that the file _rules.ov_ can now be edited by a domain expert without having to be familiar with the technology! As soon as he has changed the rules, openVALIDATION can automatically generate program code from them. This is what we call **No-Code Development**.

If you would like to try out another detailed implementation example, then have a look at the [Integration ](openvalidation-integration.md)section.

