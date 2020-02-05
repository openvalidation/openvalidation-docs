# OpenAPI Codegen

[OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) \(ehem. [Swagger](https://swagger.io/)\) und der Contract First Ansatz haben die Implementierung von REST Services revolutioniert. Bei der Entwicklung von **openVALIDATION** haben wir uns von diesem Erfolg und der Herangehensweise ein Stück weit inspirieren lassen. 

Der Contract First Ansatz besagt, dass als Erstes ein Service Contract in einer möglichst Menschen-lesbaren Notation definiert werden soll. Gleichzeitig soll die Notation bzw. die Spezifikation so formal sein, dass diese im zweiten Schritt maschinell verarbeitet werden kann. Diese grundlegende Fähigkeit ist das Fundament des Erfolgs von [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) und [OpenAPI Generator](https://openapi-generator.tech/). Es war noch nie einfacher REST bzw. Microservices und die entsprechenden Client-Proxies zu bauen. Viele Public Web API's unter anderem von Microsoft Azure, Amazon, Netflix, Deutsche Bank usw. verwenden heute sowohl OpenAPI Specification als auch die entsprechenden Codegeneratoren, um die API Economy voranzutreiben.



## Wo ist der Haken?

In so einem Service Contract werden neben den Serviceoperationen und den entsprechenden Schemata auch rudimentäre Validierungsregeln definiert. Somit lassen sich bestimmte Attribute des Schemas als Pflichtfelder definieren. Mit Hilfe regulärer Ausdrücke kann man beispielsweise das Format der E-Mail Adresse automatisch validieren. 

```yaml
openapi: "3.0.0"
info:
  version: 1.0.0
  title: sample
paths:
  /:
    post:
      requestBody:
        content:
          application/json:
            schema:
              properties:
                Name:
                  type: string
                Email:
                  type: number
                  pattern: '^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$'
              required:
                - Name
      responses:
        '200':
          description: ok

```

Es gibt auch einige weitere Abhängigkeitsprüfungen. 

Die in OpenAPI Specification integrierte Validierung basiert auf der [JSON Schema Validation](https://json-schema.org/latest/json-schema-validation.html) Spezifikation. All diese Validierungsmöglichkeiten erlauben vor allem die Validierung der Datenstruktur und des Formates eines Attributes. Komplexere Validierungsregeln mit verschachtelten Bedingungen und verschiedenen Vergleichsoperationen, die den eigentlichen Inhalt der Daten prüfen, sind nicht im Scope von JSON Schema Validation. Nichtsdestotrotz sind komplexe Validierungsregeln ein fester Bestandteil der Services bzw. der serviceorientierten Architektur. 

Das Problem ist, dass es keine Möglichkeit gibt solche komplexen Regeln in OpenAPI Specification zu deklarieren. Das Problem ist sogar noch größer. Es gibt gar keine Möglichkeit Validierungsregeln in irgendeiner standardisierten, formalen Nomenklatur zu beschreiben. Die einzig verbleibende Möglichkeit ist, die Regeln entweder in irgendeiner Programmiersprache oder in Prosa zu beschreiben. Anschließend muss ein Entwickler diese manuell in seinem Technologie Stack implementieren. 

{% hint style="warning" %}
Es gibt nach wie vor keine Möglichkeit die komplexen Validierungsregeln so formal zu beschreiben, dass daraus automatisch Programmcode in verschiedenen Programmiersprachen erzeugt werden kann.
{% endhint %}



## openVALIDATION liefert die Lösung

Wir sind der Meinung, dass das eine offene Lücke in OpenAPI Specification ist!

**Genau diese Lücke schließt openVALIDATION**. Die in der Grammatik von openVALIDATION verfassten Regeln sind formal genug, um Teil einer formalen Spezifikation zu sein. Sie sind einfach zu definieren und eignen sich optimal für den Contract First Ansatz. Gleichzeitig können diese maschinell verarbeitet werden. Diese Eigenschaften haben uns erlaubt den Standard- Funktionsumfang von OpenAPI Specification und der OpenAPI Generator zu erweitern. So weit, dass es ab jetzt möglich ist die Validierungsregeln im Service Contract zu definieren, um daraus anschließend komplette Service Stubs und Client Proxies inklusive Validierungsregeln zu generieren.

```yaml
openapi: "3.0.0"
info:
  version: 1.0.0
  title: sample
paths:
  /:
    post:
      requestBody:
        content:
          application/json:
            schema:
              properties:
                Name:
                  type: string
                Email:
                  type: number
            x-ov-rules:
              culture: en
              rule: |  
                Name HAVE to be Alex
                AND Email MUST CONTAINS @yahoo.com
      responses:
        '200':
          description: ok
```

openVALIDATION bringt eine eigene OpenAPI Extension mit, die die entsprechenden Validierungsregeln für die jeweiligen Serviceoperationen definiert. Im eigenen OV-OpenAPI Generator wird diese Extension maschinell verarbeitet, sodass daraus  Programmcode mit den entsprechenden Validierungsregeln erzeugt wird. Dieser Code wir in den Standard-Generierungsprozess von OpenAPI Generator eingefügt. Am Ende entsteht z.B. ein Java Spring Service Stub mit fertig implementierten Validierungsregeln.   


![Custom openVALIDATION-OpenAPI Generator erm&#xF6;glicht Generierung von Validierungsregeln.](../.gitbook/assets/image%20%2819%29.png)



{% hint style="info" %}
Das Schöne an OpenAPI bzw. Vendor Extensions ist, dass diese vom Standard OpenAPI Generator einfach ignoriert werden. D.h. dass man openVALIDATION Rules, wenn man möchte, nur zur Dokumentationszwecken verwenden könnte. Mit dem Bonus, dass man unter Verwendung des Custom OV-OpenAPI Generators automatisch Validierungsregeln erzeugen kann.
{% endhint %}





