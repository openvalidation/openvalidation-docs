---
description: in Arbeit . . .
---

# Beispiele

## Regeln

### WENN - DANN - Konstrukt

Das WENN - DANN - Konstrukt definiert die Bedingung und die Aktion jeder Regel.

Jede Bedingung muss ein Schlüsselwort enthalten. 

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN das Eigenkapital des Person KLEINER 30000 € ist
DANN darf der Kredit nicht vergeben werden
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Eigenkapital: 25000
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Eigenkapital: 25000
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN der Standort des Unternehmens ist NICHT München
DANN ist eine Geschäftsbeziehung schwierig
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Standort: Dortmund
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Standort: "Dortmund"
}
```
{% endtab %}
{% endtabs %}

### Alternativer Regelausdruck

{% hint style="danger" %}
Alternative Regelausdrücke beinhalten immer die [Indikator - Schlüsselwörter](https://openvalidation.gitbook.io/documentation/v/de/grammatik/schluesselwoerter#implizite-bedingungen-alternativer-regelausdruck). Die Bedingung in so einem Ausdruck enthält eine Implizite Negation!
{% endhint %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
das Alter des Antragstellers DARF NICHT KLEINER 18 sein 
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Alter: 17
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Alter: 17
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
das Alter des Versicherungsnehmers MUSS MINDESTENS 18 sein
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Alter: 16
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Alter: 16
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
der Wohnort der Person SOLL Deutschland sein 
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Wohnort: USA
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Wohnort: "USA"
}
```
{% endtab %}
{% endtabs %}

#### Implizite Bedingung

{% hint style="info" %}
Für Wahrheitswerte können implizite Bedingungen genutzt werden.
{% endhint %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript

der Vetrag MUSS unterschrieben sein

```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
unterschrieben: false
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    unterschrieben: false
}
```
{% endtab %}
{% endtabs %}

### Multiple Bedingungen

#### UND

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN das Alter des Bewerbers GRÖßER 70 ist
UND sein Wohnort ist NICHT Dortmund
DANN Sie dürfen nicht älter als 70 sein und müssen aus Dortmund kommen
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Wohnort: Berlin
Alter: 18
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Wohnort: "Berlin",
    Alter: 18
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
    das Alter des Bewerbers DARF NICHT KLEINER 25 sein
UND sein Beruf MUSS Berater sein
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Alter: 18
Beruf: Berater
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Alter: 18,
    Beruf: "Berater"
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
    der Bewerber DARF KEIN Student sein
UND sein Alter MUSS MINDESTENS 25 Jahre sein
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Student: false,
Alter: 24
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Student: false,
    Alter: 24
}
```
{% endtab %}
{% endtabs %}

#### ODER

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN der Abschluss des Bewerbers NICHT Bachelor ist
ODER seine Berufserfahrung WENIGER als 5 Jahre ist
DANN Sie müssen einen Bachelor oder 5 Jahre Erfahrung haben
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Abschluss: Bachelor
Berufserfahrung: 4
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Abschluss: "Bachelor",
    Berufserfahrung: 4
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
     der Abschluss des Bewerbers MUSS Bachelor sein
ODER seine Berufserfahrung MUSS MINDESTENS 5 Jahre sein
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Abschluss: Bachelor
Berufserfahrung: 4
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Abschluss: "Bachelor",
    Berufserfahrung: 4
}
```
{% endtab %}
{% endtabs %}

### Komplexe Bedingung

{% hint style="warning" %}
Bei komplexen Regeln müssen die Einrückungen beachtet werden.
{% endhint %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN der Wohnort der Person IST London
 UND der Name des Person ist GLEICH Mycroft Holmes
     ODER sein Name ist GLEICH Sherlock Holmes
DANN die Person ist ein Genie
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Wohnort: London
Name: Sherlock Holmes
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Wohnort: "London",
    Name: "Sherlock Holmes"
}
```
{% endtab %}
{% endtabs %}

## Arithmetische Beispiele

Die Grammatik unterstützt arithmetische bzw. mathematische Operationen. 

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN Aktiva - Passiva NICHT GLEICH 0 
DANN die Bilanz muss immer ausgeglichen sein
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Aktiva: 10
Passiva: 9
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Aktiva: 10,
    Passiva: 9
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
WENN das Eigenkapital / Bilanzsumme NICHT GLEICH Eigenkapitalquote
DANN die Bilanz enthält Fehler 
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Eigenkapital: 10
Bilanzsumme : 9
Eigenkapitalquote: 2
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Eigenkapital: 10,
    Bilanzsumme : 9,
    Eigenkapitalquote: 2
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Regel" %}
```coffeescript
 die Berufserfahrung von Alter - 18 Jahren DARF NICHT GERINGER sein als 10 
```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Alter: 17
Berufserfahrung: 10
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Alter: 17,
    Berufserfahrung: 10
}
```
{% endtab %}
{% endtabs %}

## Variablen

Numerische Werte

{% tabs %}
{% tab title="Variablen-Deklaration" %}
```coffeescript
    42
ALS Antwort auf Alles
```
{% endtab %}
{% endtabs %}

Zeichenketten

{% tabs %}
{% tab title="Variablen-Deklaration" %}
```coffeescript
    Honig
ALS lecker
```
{% endtab %}
{% endtabs %}

Wahrheitswerte

{% tabs %}
{% tab title="Variablen-Deklaration" %}
```coffeescript
    das Alter der Bewerberin ist KLEINER als 18
ALS minderjährig
```
{% endtab %}
{% endtabs %}

Mit Variablen können auch Berechnungen durchgeführt werden. 

{% tabs %}
{% tab title="Variablen" %}
```coffeescript
    
    25
ALS Berufseinstieg

    Berufseinstieg - Abitur
ALS Studienzeit
    
    Alter - Berufseinstieg
ALS Berufserfahrung

    Berufserfahrung * 12
ALS Berufserfahrung in Monaten

```
{% endtab %}

{% tab title="YAML Schema" %}
```yaml
Alter: 20,
Abitur: 19
```
{% endtab %}

{% tab title="JSON Schema" %}
```javascript
{
    Alter: 20,
    Abitur: 19
}
```
{% endtab %}
{% endtabs %}

## Kommentare

{% tabs %}
{% tab title="First Tab" %}
```coffeescript
KOMMENTAR Das ist ein Kommentar
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="First Tab" %}
```coffeescript
KOMMENTAR Mehrzeilige Kommentare
          sind auch möglich
```
{% endtab %}
{% endtabs %}

## OpenAPI

[openVALIDATION ](https://docs.openvalidation.io/v/de/)bringt eine eigene OpenAPI Erweiterung mit, die die entsprechenden Validierungsregeln für die jeweiligen Serviceoperationen definiert.

{% tabs %}
{% tab title="oapi.spec.yaml" %}
```coffeescript
openapi: "3.0.0"
info:
  version: 1.0.0
  title: openVALIDATION
  description: openVALIDATION Beispiele.
paths:
  /:
   post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/person'
            x-ov-rules:
              culture: de
              rule: |

                  KOMMENTAR Das ist ein Kommentar
              
                  WENN das Eigenkapital des Person KLEINER 30000 € ist
                  DANN darf der Kredit nicht vergeben werden
                  
                  
     responses:
        '200':
          description: success
components:
  schemas:
    person:
      type: object
      properties:
        eigenkapital:
          type: integer   

```
{% endtab %}

{% tab title="Download" %}
{% file src=".gitbook/assets/oapi.spec \(1\).yaml" caption="oapi.spec.yaml" %}
{% endtab %}
{% endtabs %}



