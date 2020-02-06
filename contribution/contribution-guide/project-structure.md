---
description: An dieser Stelle wird der Aufbau des core Projektes erläutert
---

# Project structure

openVALIDATION ist in Java 8 als ein Maven multi-module Projekt realisiert. Das Repository ist auf GitHub unter [https://github.com/openvalidation/openvalidation](https://github.com/openvalidation/openvalidation) veröffentlicht.

![Das Projekt ist in einzelne Module aufgeteilt](../../.gitbook/assets/image%20%2831%29.png)

### openvalidation-antlr

In diesem Modul ist die Parsing Routine implementiert. Das initiale Parsen des Textes indem Validierungsregeln enthalten sind übernimmt das Framework ANTLR. 

In der Datei main.g4 befindet sich die ANTLR Grammatik. Mit dem entsprechendem ANTLR Plugin [antlr4-maven-plugin](https://www.antlr.org/api/maven-plugin/latest/usage.html) wird Parser Code generiert. Dieser Code Ladet beim Erstellen des Projektes im Target Ordner und wird automatisch mit dem Plugin [build-helper-maven-plugin](https://www.mojohaus.org/build-helper-maven-plugin/usage.html) in das Projekt eingebunden.

{% hint style="warning" %}
Das Projekt sollte am besten nach jedem 

`git pull`

mit

`mvn clean package`

neue gebaut werden, damit das ANTLR Plugin aus der **main.g4** Grammatik die entsprechenden Parser Klassen generieren kann. Somit wird sichergestellt, dass die möglichen Änderungen an der Grammatik selbst aktualisiert werden.
{% endhint %}

![](../../.gitbook/assets/image%20%2842%29.png)

Die meiste Implementierung des Moduls **openvalidation-antlr** beschäftigt sich mit der Erstellung des Abstract Syntax Trees. Dazu muss zuerst der durch ANTLR Parser geparste **Parse Tree** in einen initialen **AST** transformiert werden. Anschließend muss der **AST** mit den Postprozessoren finalisiert werden. 

Mehr zu diesem Thema wird im Bereich [Architektur/Parser](architecture.md#parser) erklärt.



### openvalidation-cli

Das Modul zum Ausführen von openVALIDATION in der Konsole als eine Kommandozeilen-Anwendung. 

Beim Bauen des CLI Projektes wird mit Hilfe des Plugins[ spring-boot-maven-plugin](https://docs.spring.io/spring-boot/docs/current/maven-plugin/usage.html) eine self contained executable jar erzeugt.  Alle Abhängigkeiten, CLASSPATHES und Sonstiges sind in der JAR embedded und müssen nicht mehr vom Anwender von außen mitgegeben werden. 

{% hint style="warning" %}
Das CLI Projekt gehört eigentlich nicht zum Kern von openVALIDATION und wird deshalb mittelfristig in ein eigenes Repository verschoben.
{% endhint %}



### openvalidation-common

xxx



### openvalidation-core

xxx



### openvalidation-generation

xxx



### openvalidation-integration-tests and openvalidation-integration-generator

xxx



