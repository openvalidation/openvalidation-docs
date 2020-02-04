# Best Practice

Damit das Erstellen von Validierungsregeln in openVALIDATION möglichst einfach und effizient abläuft, sollte einiges beachtet werden.

## Einrichten von openVALIDATION

![](.gitbook/assets/image%20%2855%29.png)

Zielgruppe, die sich primär mit der Erstellung der Regeln befassen soll sind Fachleute wie:

* Domain Experten
* Business Analysten 
* Requirement Engineers
* In besonderen Use-Cases können es auch die Endanwender selbst sein

Um mit der Erstellung der Validierungsregeln beginnen zu können, muss als Erstes die CLI und der erzeugte Code von einem Techniker, am besten von einem Entwickler, eingerichtet bzw. in die Entwicklungsumgebung integriert werden.

Nach einer erfolgreichen Setup Phase, sollten die Fachleute das Erstellen der Regeln in einer von der Technik isolierten Umgebung betreiben. Dazu reicht bereits ein beliebiger Texteditor, mit dem man das Regelwerk bearbeiten und abspeichern kann.

Das gesamte Regelwerk sollte in einer bzw. mehreren Dateien mit der Endung **\*.ov** untergebracht werden.



## Der Aufbau des Regelwerkes

Das Regelwerk besteht üblicherweise aus mehreren Validierungsregeln und weiteren zusätzlichen strukturellen Elementen, wie:

* Variablen
* IMPORTS
* KOMMENTAREN
* Domainspezifischen Operatoren \(in Arbeit…\)

Sie alle dienen einem einzigen Zweck. Sie halten die Komplexität im Zaum und vereinfachen somit die Erstellung bzw. Änderung von Validierungsregeln. 

Das Regelwerk sollte so aufgebaut sein, dass die technische Komplexität möglichst weit von der natürlichen Definition einer Regeln abstrahiert wird.

In der Softwareentwicklung aber auch insgesamt in der Welt der Technik ist es üblich, die Komplexität in mehrere Schichten aufzuteilen. Die untere, also die technische Schicht, wird in der Regel als Low Level bezeichnet. Die höhere, weit von der Technik abstrahierte Schicht bezeichnet man dagegen als High Level.  



## Erstellung der Regeln

Das Regelwerk in openVALIDATION sollte diesem grundlegenden Prinzip folgen. Die oberste Schicht des Regelwerks sind die Regeln selbst. Die Regeln sollten daher möglichst natürlich formuliert werden. 

Die Regeln sind **atomar**. Sie sollten keinerlei Abhängigkeiten zu anderen Regeln enthalten. Die Reihenfolge der Regeln ist ebenfalls irrelevant.

```coffeescript
das Alter des Bewerbers DARF NICHT KLEINER als 18 sein  

der Ort des Bewerbers MUSS Dortmund sein
```



## Vorbedingungen

Vorbedingungen können in Form von Variablen definiert werden. Eine häufig verwendete Bedingung ist beispielsweise die Abfrage, ob der Bewerber bereits 18 Jahre alt ist. Diese kann man hinter einer einfachen Variablen namens "Minderjährig" auslagern. Diese Variable kann man wiederum als Vorbedingung in verschiedenen Regeln verwenden. 

```coffeescript
     das Alter des Bewerbers ist KLEINER 18
ALS  Minderjährig

     der Bewerber DARF NICHT Minderjährig sein
UND  sein Ort MUSS Dortmund heißen

WENN der Bewerber Minderjährig ist
ODER seine Berufserfahrung ist KÜRZER als 2 Jahre
DANN Es fehlt Ihnen an Erfahrung    
```

Somit reduziert man die Komplexität und erhöht die Lesbarkeit der eigentlichen Regeln. 

{% hint style="info" %}
Grundsätzlich gilt die Regel, sobald eine Bedingung in mehreren Regeln verwendet wird, dann ist es eine Vor- oder Teilbedingung und sollte in eine wiederverwendbare Variable ausgelagert werden.
{% endhint %}



## Die Namen der Variablen

Variablen sind sehr nützliche Konstrukte. Sie erhöhen die Lesbarkeit und Natürlichkeit einer Validierungsregel immens. Auch, wenn in dem Namen einer Variablen Leerzeichen und weitere Sonderzeichen erlaubt sind, sollte man einen möglichst aussagekräftigen und flexiblen Namen wählen.

```coffeescript
    die Berufserfahrung des Bewerbers ist LÄNGER als 10 Jahre
ALS Senior


der Bewerber MUSS ein Senior sein

der Bewerber DARF NICHT ein Senior sein

```





