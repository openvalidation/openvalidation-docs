---
description: Beschreibung der CLI - Nutzung
---

# Command Line Interface

## Voraussetzungen

Für die Nutzung von **openValidation** wird Java ab Version 8 benötigt. Hier kann das entsprechende [JRE 1.8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) heruntergeladen werden.

Ein Befehlsspracheninterpreter \(CLI\) ist ein Mittel zur Interaktion mit der **openValidation.jar** , bei dem der Benutzer Befehle an das Programm in Form von aufeinanderfolgenden Textzeilen \(Befehlszeilen\) eingibt. 

**openValidation.jar** ist ein einfaches Java Command Line Interface, welches man [hier](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar) herunterladen kann.

 

## Optionen

Die Optionen geben die grundlegende Aktion an, die **openValidation.jar** ausführen soll und liefern zusätzliche Informationen darüber, wie der Befehl funktionieren soll.

 Hier sind die möglichen Optionen aufgelistet:

| Parameter        | Abkürzung | Beschreibung |
| :--- | :--- | :--- |
| --rule | `-r`  | Hier werden die **Validierungsregeln** definiert, die mit Hilfe von **openValidation** erstellt werden sollen |
| --schema | `-s` | Das **Schema** definiert die Datenstruktur des Datensatzes |
| --culture | `-c` | Diese Option definiert die **natürliche Sprache,** in welcher die Regeln definiert wurden |
| --language | `-l` | Diese Option definiert die **Programmiersprache**, in welcher der Code generiert werden soll |
| --output | `-o` | Die Output-Option definiert ein Verzeichnis, in dem die generierten Code-Dateien gespeichert werden. Ohne Angabe des Output-Parameters wird der generierte Code lediglich in der Console ausgegeben |
| --single-file | `-f` | Durch diese Option wird der Output als eine einzige Datei gespeichert. Diese Option ist abhängig vom angegeben Pfad für die Speicherung des **Outputs** |
| --params | `-p` | Eingabe bestimmter Parameter |
| --help | `-h` | Hilfe zu der **openValidation -** CLI aufrufen |
| --verbose | `-v` | Gibt die Logs von **openValidation** aus |
| --al | `-a` | Diese Option gibt die Liste aller **verfügbaren Programmiersprachen** aus. |
| --no-banner | `-n` | Diese Option bestimmt die Anzeige des Banners. |
| ~~~~ |  |  |

### 

### Rule

Mit der Hilfe der Option **Rule \(`-r`\)** kann die Validierungsregel angegeben werden. Diese muss ein bestimmtes Format haben, um für die Software nutzbar zu sein.

Außerdem ist dies eine **Pflicht**-Option**.** Ohne Validierungsregel kann schließlich auch kein sinnvoller Code generiert werden. 

Jede eingegebene Regel soll einen bestimmten Aufbau haben, um für **openValidation** lesbar zu sein.

Der Aufbau wird explizit durch die [Grammatik ](https://openvalidation.gitbook.io/documentation/grammatik)definiert die von **openValidation** genutzt wird. Die Grammatik teilt die Eingaben in vier möglichen Elemente ein, die in einer Eingabe durch einen Absatz \(doppelte Leerzeile\) getrennt werden müssen.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
     -s "{Nachricht : \"Text\"}" 
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
     -s "{Nachricht : \"Text\"}" 
```
{% endtab %}
{% endtabs %}

Die Regeln können als reiner Text geschrieben werden. Außerdem besteht die Möglichkeit des Einfügens einer **URL** oder eines **Pfades** zu einer Datei mit diversen Validierungsregeln. Wenn es mehrere Regel in einem gleichen Kontext geben soll, wird empfohlen diese in einer **Datei** bzw. **URL** abzuspeichern. Bei einem Verweis auf eine Datei wird empfohlen, die Datei in **UTF-8** zu formatieren.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r C:/Pfad_zum_Ordner/regeln.txt ^
     -s "{Wohnort : \"Berlin\"}" 
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r /home/Pfad_zum_Ordner/regeln.txt \
     -s "{Wohnort : \"Berlin\"}" 
```
{% endtab %}
{% endtabs %}

### Schema

Mit Hilfe der Option **Schema \(`-s`\)** kann das Schema definiert werden. Dadurch kann **openValidation** erkennen, welche Eigenschaften und Typen für den Output benötigt werden. 

Außerdem ist dies eine **Pflicht**-Option. Ohne Eingabe des Schemas kann kein Code generiert werden kann.

Das Schema wird im **JSON**-Format angegeben. Bei einem Verweis auf eine Datei wird empfohlen die Datei in **UTF-8** zu formatieren.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN der Typ DES Fahrzeug IST Fahrrad DANN Fahrradkuriere sind erwünscht" ^
     -s "{Fahrzeug: {Typ : \"Fahrrad\"}}"
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN der Typ DES Fahrzeug IST Fahrrad DANN Fahrradkuriere sind erwünscht" \
     -s "{Fahrzeug: {Typ : \"Fahrrad\"}}" 
```
{% endtab %}
{% endtabs %}

Um Fehler bei der Eingabe des Schemas zu vermeiden, wird empfohlen, den Pfad zu dem gewünschten Schema anzugeben.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN der Typ DES Fahrzeug IST Fahrrad DANN Fahrradkuriere sind erwünschscht" ^
     -s C:/Pfad_zum_Ordner/schema.json
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN der Typ DES Fahrzeug IST Fahrrad DANN Fahrradkuriere sind erwünscht" \
     -s /home/Pfad_zum_Ordner/schema.json
```
{% endtab %}
{% endtabs %}

### Culture

Mit Hilfe der Option **Culture \(`-c`\)** kann die natürliche Sprache angegeben werden, in der die Validierungsregeln verfasst wurden. 

Diese Option ist keine Pflicht. Wenn keine Angaben zu der natürlichen Sprache vorliegen, wird die Sprache des Betriebssystems / Nutzers benutzt.

Wichtig ist dabei zu beachten, dass die Sprache der Validierungsregel auch der _natürlichen Sprache_ entspricht, die mit der **Culture**-Option festgelegt wird.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt!" ^
     -s "{Nachricht : \"Text\"}" ^
     -c de
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt!" \
     -s "{Nachricht : \"Text\"}" \
     -c de
```
{% endtab %}
{% endtabs %}

#### Sprachen

Folgende Sprachen werden zurzeit unterstützt:

| Sprache | Befehl |
| :--- | :--- |
| Deutsch | `-c de` |
| Englisch | `-c en` |
| Russisch | `-c ru` |

```java
java -jar openvalidation.jar ^
     -r "IF the Message EQUALS hallo THEN Hello World" ^
     -s "{Message : \"Text\"}" ^
     -c en
```

### Language

Mit der Option **Language \(`-l`\)** kann die Programmiersprache des Outputs angegeben werden. 

Diese Option ist keine Pflicht. Wenn die Programmiersprache nicht explizit angegeben wurde, wird standardmäßig **Java** als Output ausgegeben. 

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
     -s "{Nachricht : \"Text\"}" ^
     -l javascript 
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
     -s "{Nachricht : \"Text\"}" \
     -l csharp
```
{% endtab %}
{% endtabs %}

#### Programmiersprachen

Folgende Programmiersprachen werden zurzeit unterstützt:

| Programmiersprache | Befehl |
| :--- | :--- |
| Java | `-l java` |
| C\# | `-l csharp` |
| JavaScript | `-l javascript` |
| Python | `-l python` |

Außerdem können **alle verfügbaren Programmiersprachen** durch die Option **`--al` \(`-a`\)** ausgegeben werden. Diese Option kann auch ohne Eingabe anderer Optionen bzw. **Pflicht**-Optionen genutzt werden.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar /
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

Mit Hilfe der Option **Output \(`-o`\)** kann der Output in einer Datei oder einem Ordner gespeichert werden. Wenn diese Option nicht definiert wurde, wird die Ausgabe nur in der Konsole ausgegeben.

Festlegung des Pfades für Dateien und Ordner zum Speichern des generierte Codes.

In diesen Pfad werden die **Regel** und das **Framework** in der entsprechenden Programmiersprache generiert. Es werden immer zwei Dateien generiert.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
     -s "{Nachricht : \"Text\"}" ^
     -o C:/Pfad_zum_Ordner
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
     -s "{Nachricht : \"Text\"}" \
     -o /home/Pfad_zum_Ordner
```
{% endtab %}
{% endtabs %}

### Single File

Um den Output auf eine Datei zu reduzieren, gibt es die Option **Single-File \(`-f`\)**. Durch Aktivierung dieser Option wird nur eine einzelne Datei erstellt. Diese enthält die Regel und das dazugehörige Framework.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
     -s "{Nachricht : \"Text\"}" ^
     -o C:/Pfad_zum_Ordner ^
     -f
```
{% endtab %}

{% tab title="Unix" %}
```text
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
     -s "{Nachricht : \"Text\"}" \
     -o /home/Pfad_zum_Ordner \
     -f
```
{% endtab %}
{% endtabs %}

### Verbose

Mit Hilfe der Option **Verbose \(`-v`\)** kann zusätzlicher Output ausgegeben werden. Dieser hilft dabei, den generierten Code und zusätzlich die ausgeführten Schritte besser zu verstehen.

Außerdem kann jeder Schritt der Generierung genau analysiert werden. Diese Option erleichtert das Finden von Fehlern und somit die Verifizierung möglicher falscher Eingaben.

Zusätzlich zu allen Eingaben wird auch jeder Schritt des gesamten Prozesses dargestellt.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
     -s "{Nachricht : \"Text\"}" ^
     -v
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
     -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
     -s "{Nachricht : \"Text\"}" \
     -v
```
{% endtab %}
{% endtabs %}

### Params

Mit der Hilfe der Option **Params \(`-p`\)** kann man den generierten Code customizen. Mehrere Parameter werden im Format **name**=**value** und durch Semikolon getrennt eingegeben.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
   -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
   -s "{Nachricht : \"Text\"}" ^
   -p model_type=io.openvalidation.Data;^
      generated_class_namespace=io.openvalidation.rules;^
      generated_class_name=MyRuleSet
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
   -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
   -s "{Nachricht : \"Text\"}" \
   -p model_type=io.openvalidation.Data;\
      generated_class_namespace=io.openvalidation.rules;\
      generated_class_name=MyRuleSet
```
{% endtab %}
{% endtabs %}

**model\_type** \(default=leer\)  
ist ein kompletter\(package + classname\) Name des externen Typen des Models. Diese Angabe wird zum deklarieren des Models innerhalb des generierten Codes verwendet.

**generated\_class\_namespace** \(default=io.openvalidation.rules\)  
ist der Name des packages der generierten Klassen. Sowohl das Framework als auch die Implementierung\(das eigentlich Rule Set\) bekommen diesen Packagenamen zugewiesen. 

**generated\_class\_name** \(default=HUMLValidator\)  
ist der Name der generierten Klasse, die die eigentliche Implementierung des Rule Sets enthält.

### No Banner

Mit der Hilfe der Option **No-Banner \(`-n`\)** kann der Banner aus der Konsole entfernt werden.

{% tabs %}
{% tab title="Windows" %}
```java
java -jar openvalidation.jar ^
   -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" ^
   -s "{Nachricht : \"Text\"}" ^
   -n true
```
{% endtab %}

{% tab title="Unix" %}
```java
java -jar openvalidation.jar \
   -r "WENN die Nachricht IST GLEICH hallo DANN Hallo Welt" \
   -s "{Nachricht : \"Text\"}" \
   -n true
```
{% endtab %}
{% endtabs %}

### Help

Durch Nutzung der Option **Help \(`-h`\)** kann die Hilfe ausgegeben werden. 

Diese Option bietet Hilfestellungen für den Umgang mit der **openValidation - CLI** an.

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

