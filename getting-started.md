# Startklar in 3 min.

### Voraussetzungen

Du benötigst Java ab der Version 8. Hier kannst Du das entsprechende [JRE 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) herunterladen.

### 1. Installation

Du benötigst **openvalidation.jar** um Code generieren zu können. **openvalidation.jar** ist ein einfaches Java Command Line Interface. Die **openvalidation.jar** kannst Du hier herunterladen.[![](.gitbook/assets/button1%20%285%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar) 

 

{% tabs %}
{% tab title="Windows" %}
Am besten Du erstellst ein neues Verzeichnis unterhalb deines persönlichen Temp Ordners und lädst die **openvalidation.jar** in diesen Ordner herunter:

```bash
cd %temp%
mkdir openvalidation
cd openvalidation
curl http://download.openvalidation.io/openvalidation.jar
```
{% endtab %}

{% tab title="Unix" %}
Am besten Du erstellst ein neues Verzeichnis unterhalb deines persönlichen Temp Ordners und lädst die **openvalidation.jar** in diesen Ordner herunter:

```bash
cd tmp
mkdir openvalidation
cd openvalidation
curl http://central.maven.org/maven2/openvalidation.jar
```
{% endtab %}
{% endtabs %}

### 2. Eine Regel erstellen

Ok, jetzt bist Du startklar und kannst Deine erste Validierungsregel in openVALIDATION programmieren. Erstelle dazu eine Datei namens _Regeln.ov_  und füge den folgenden Inhalt ein: 

{% code title="Regeln.ov" %}
```coffeescript
WENN die Nachricht ist GLEICH Hallo
DANN Hallo openVALIDATION!
```
{% endcode %}

_Diese Regel ist so formatiert, dass die Schlüsselwörter und Syntax der openVALIDATION-Sprache am besten darin zu erkennen sind. Du kannst aber auch einfach `Wenn die Nachricht gleich Hallo ist dann Hallo openVALIDATION!` schreiben und es würde ebenso gut funktionieren._

Neben der eigentlichen Regel benötigst Du noch ein Schema. Das Schema definiert die Datenstruktur des Datensatzes welches validiert werden soll. Du kannst ein einfaches JSON-Objekt verwenden, aus dem dann automatisch ein JSON Schema abgeleitet wird. Unser Schema besteht lediglich aus einem Textfeld namens "Nachricht".

{% tabs %}
{% tab title="YAML" %}
```yaml
Nachricht: text
```
{% endtab %}

{% tab title="JSON" %}
{% code title="" %}
```scheme
{
    Nachricht : "Text"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### 3. Code generieren

Da unsere Validierungsregel und das entsprechende Schema bereits feststehen, können wir jetzt den entsprechenden Code generieren. 

{% tabs %}
{% tab title="Windows" %}
```bash

java -jar openvalidation.jar ^
     -r Regeln.ov ^
     -s "{Nachricht : \"Text\"}" ^
     -l javascript ^
     -o meineRegeln.js ^
     -c de ^
     -f

```
{% endtab %}

{% tab title="Unix" %}
```bash

java -jar openvalidation.jar \
     -r Regeln.ov \
     -s "{Nachricht : \"Text\"}" \
     -l javascript \
     -o meineRegeln.js \
     -c de \
     -f
     
```
{% endtab %}
{% endtabs %}

| Parameter        | Beschreibung |
| :--- | :--- |
| -r | Über den Parameter **Regel** wird die eigentliche Validierungsregel entweder direkt oder als Dateiverweis übergeben. In unserem Fall übergeben wir die Datei _Regeln.ov_ |
| -s | Durch das Schema wird openVALIDATION die Datenstruktur übermittelt |
| -l | Die zu übersetzende Programmiersprache  |
| -c | Der Culture Code der natürlichen Sprache \(**de** für Deutsch/ **en** für Englisch\) |
| -o | Output Parameter definiert die Code-Datei. Alternativ kann man auch ein Verzeichnis angeben indem die Code-Dateien nach dem Generierungsvorgang abgelegt werden. Ohne Angabe des Output Parameters wird der generierte Code lediglich in der Console ausgegeben.  |
| -f | Dieser Parameter \(--single-file\) sorgt dafür, dass der generierte Code in einer Datei erzeugt wird. Normalerweise gibt es zwei Dateien: das Basisframework und die Implementierung der Regeln. |

Glückwunsch! Du hast soeben Deine erste Validierungsregel in JavaScript erzeugt. Diese befindet sich in der Datei _meineRegeln.js_**.** So sieht das Resultat aus:

{% code title="" %}
```javascript
...

huml.appendRule("",
     ["Nachricht"],
     "Hallo openVALIDATION!",
     function(model) { return  huml.EQUALS(model.Nachricht, "Hallo");},
     false
);

...
```
{% endcode %}



### 4. Regeln integrieren

Jetzt möchten wir ausprobieren, ob unsere Regel auch funktioniert. Dazu erstellen wir eine Datei namens _test.html_  und fügen den folgenden Code hinzu:

{% code title="test.html" %}
```markup
<html>
  <body>

    <input id="message" />
    <div id="error"></div>

    <script src="meineRegeln.js"></script>
    <script type="text/javascript">

      window.addEventListener('load', (event) => {

        var validator = new HUMLValidator();

        document.getElementById('message').onkeyup = function(e){
            var result = validator.validate({Nachricht:e.target.value});

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

Wir rufen die **test.html** im Browser auf und geben _"Hallo"_  in das Eingabefeld ein:

![](.gitbook/assets/image%20%2816%29.png)

Wie erwartet, kommt bei der Eingabe des Wertes "_Hallo"_  unsere Fehlermeldung. 



### 5. No-Code Development

Du kannst jetzt die Regeln erweitern, indem Du die Datei _Regeln.ov_  mit weiteren Regeln befüllst und anschließend nochmal den Generierungsprozess mittels openVALIDATION CLI durchführst. Orientiere Dich dazu einfach an den Schritten 2 bis 4.

Das Schöne daran ist, dass die Datei _Regeln.ov_  ab jetzt von einem Fachexperten bearbeitet werden kann, ohne dass er sich mit der Technik auskennen muss! Sobald er die Regeln geändert hat, kann openVALIDATION daraus automatisch Programmcode erzeugen. Das nennen wir **No-Code Development**.

Wenn Du ein weiteres ausführliches Implementierungsbeispiel ausprobieren möchtest, dann schau doch mal im Bereich [Integration](openvalidation-integration.md) nach.

