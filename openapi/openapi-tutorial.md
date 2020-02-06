# OpenAPI Tutorial

Um die Funktionsweise des OV-OpenAPI Codegenerators zu demonstrieren, sollten wir gemeinsam ein Beispiel umsetzen. 

1. Dazu erstellen wir zunächst einen Service Contract in OpenAPI Specification. 
2. Danach erweitern wir den Service Contract um Validierungsregeln. 
3. Wir generieren einen Service Stub anhand des Service Contracts 
4. Testen diesen mit Postman. 
5. Und, falls uns am Ende langweilig werden sollte, erstellen wir aus demselben Service Contract einen JavaScript Client, der die Validierungsregeln clientseitig ausführen wird, bevor die Daten an den Service gesendet werden.  

![](../.gitbook/assets/image%20%2840%29.png)

Für dieses Beispiel werden gewisse Kenntnisse wie z.B. OpenAPI, NPM und Grundlagen der modernen Frontend-Entwicklung vorausgesetzt. Des Weiteren müssen folgende technische Voraussetzungen erfüllt werden:

Java Version                                          [1.8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)  
NodeJS \(inkl. npm\) ab der Version     [8.5](https://nodejs.org/en/download/)  
Browserify                                              [LATEST](http://browserify.org/#install)  
Postman                                                 [LATEST](https://www.getpostman.com/downloads/)  
  
ov-openapi-generator-cli.jar               [![](../.gitbook/assets/button2%20%283%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-openapi-generator/ov-openapi-generator-cli.jar)  




## 1. Service Contract in OpenAPI erstellen

Erstellen wir zunächst ein neues Arbeitsverzeichnis:

{% tabs %}
{% tab title="Windows" %}
`WINDOWS TASTE` drücken, "CMD" eingeben und mit `RETURN` die Konsole öffnen.  
Projektverzeichnis erstellen

```bash
mkdir C:\tmp\ov-openapi
cd C:\tmp\ov-openapi
```
{% endtab %}

{% tab title="Unix" %}
Öffne  dazu Deine Shell und gib die folgenden Befehle ein:

```bash
mkdir C:\tmp\ov-openapi
cd C:\tmp\ov-openapi
```
{% endtab %}
{% endtabs %}

In dem Arbeitsverzeichnis erstellen wir eine neue OpenAPI Spezifikation

{% tabs %}
{% tab title="Windows" %}
```bash
echo > oapi.spec.yaml
```
{% endtab %}

{% tab title="Unix" %}
```bash
echo > oapi.spec.yaml
```
{% endtab %}
{% endtabs %}

Wir öffnen die oapi.spec.yaml mit einem beliebigen Texteditor. In diesem Beispiel verwenden wir dazu die **atom** IDE. Öffnen wir also das zuvor erstellte Projektverzeichnis mit atom.

```bash
atom . oapi.spec.yaml
```

Jetzt fügen wir folgenden Inhalt in unsere Spezifikation ein:

{% code title="oapi.spec.yaml" %}
```yaml
openapi: "3.0.0"
info:
  version: 1.0.0
  title: openVALIDATION Beispiel
  description: openVALIDATION Integrationsbeispiel.
paths:
  /:
   post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Bewerber'
      responses:
        '200':
          description: success
components:
  schemas:
    Bewerber:
      type: object
      properties:
        Name:
          type: string
        Alter:
          type: integer
        Ort:
          type: string

```
{% endcode %}

Unser Service Contract definiert einen **REST Service** mit einer **POST Operation**. Diese Service Operation nimmt eine komplexe Datenstruktur, die durch das **Schema Bewerber** definiert wird, entgegen. Das Schema selbst besteht lediglich aus 3 Attributen: **Name, Alter** und **Ort**. 



## 2. Validierungsregel in den Service Contract hinzufügen

Jetzt möchten wir unserem Service Contract eine bestimmte Validierungsregel mitgeben. Sagen wir mal, dass der Ort des Bewerbers immer Dortmund sein muss. Dazu fügen wir eine neue Definition namens `x-ov-rule` direkt hinter dem Verweis auf das Schema ein \(Zeile 14\). Und so sieht jetzt unser fertiger Service Contract aus:

{% code title="oapi.spec.yaml" %}
```yaml
openapi: "3.0.0"
info:
  version: 1.0.0
  title: openVALIDATION Beispiel
  description: openVALIDATION Integrationsbeispiel.
paths:
  /: 
   post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Bewerber'
            x-ov-rules:
              rule: |
                der Ort des Bewerbers MUSS Dortmund sein
      responses:
        '200':
          description: success
components:
  schemas:
    Bewerber:
      type: object
      properties:
        Name:
          type: string
        Alter:
          type: integer
        Ort:
          type: string
```
{% endcode %}

Nun sind wir startklar und können den eigentlichen REST Service generieren.



## 3. Generieren des Service Stub's als Java Spring Boot

Zum Generieren des Service Stubs verwenden wir den Custom OpenAPI Generator von openVALIDATION. Dieser Generator ist eine Java CLI Anwendung, die in Form einer JAR Datei namens **`ov-openapi-generator-cli.jar`** vorliegt. Die Generierung erfolgt durch folgenden CLI Aufruf:

{% tabs %}
{% tab title="Windows" %}
```bash
java -jar ov-openapi-generator-cli.jar generate ^
     -g ov-java-spring-server ^
     -i oapi.spec.yaml ^
     -o out/server
```
{% endtab %}

{% tab title="Unix" %}
```bash
java -jar ov-openapi-generator-cli.jar generate \
     -g ov-java-spring-server \
     -i oapi.spec.yaml \
     -o out/server
```
{% endtab %}
{% endtabs %}

Mit dem Parameter **`-g`** `ov-java-spring-server` teilt man dem OpenAPI Generator mit, dass der spezielle openVALIDATION Generator namens **ov-java-spring-server** verwendet werden soll. Dieser Generator erweitert die Standardfunktionalität des bereits vorhandenen OpenAPI Generators namens **Spring** um Generierung zusätzlicher Validierungsregeln, die wiederum hinter der OpenAPI Extension **x-ov-rules** definiert sind. 

Nach einem erfolgreichen Generierungsvorgang ist eine neue Ordnerstruktur direkt im Arbeitsverzeichnis entstanden \(**out/server\)**. Dort befindet sich unser generierter Service Stub. Genaugenommen ist ein Maven Projekt entstanden, welches wir mit einer Java-fähigen IDE, wie z.B. IntelliJ öffnen können. 

Wir können aber auch sofort loslegen und das generierte Projekt einfach kompilieren und starten:

```bash
cd out/server
mvn clean package
java -jar target/openapi-spring-1.0.0.jar
```

Wenn alles geklappt hat, läuft der Service bereits am Port 8080 und wir können ihn einfach mal im Browser unter [http://localhost:8080/](http://localhost:8080/) aufrufen:

![](../.gitbook/assets/image%20%288%29.png)

{% hint style="info" %}
Der openVALIDATION Generator enthält die gesamte Standard-Funktionalität des Original OpenAPI Generators und fügt lediglich die Verarbeitung der **x-ov-rules** Extension hinzu. Diese Verarbeitung umfasst unter anderem das Generieren der Validierungsregeln und deren Integration in das jeweilige Framework, wie z.B. in Spring Boot. Das Customizing ist gemäß dem offiziellen Customizing-Guide \([https://openapi-generator.tech/docs/customization.html](https://openapi-generator.tech/docs/customization.html)\) des OpenAPI Generators implementiert.
{% endhint %}



## 4. Den Service und die Validierungsregel testen

Zum Testen des Services kann man selbstverständlich die integrierte OpenAPI bzw. Swagger Testpage verwenden. Wir können aber auch mit einem anderen **REST**-fähigen Tool, wie z.B. Postman, einen POST Request absenden. So sieht unser Request aus:

{% tabs %}
{% tab title="YAML" %}
```yaml
"Ort" : Berlin
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
    "Ort" : "Berlin"
}
```
{% endtab %}
{% endtabs %}

![](../.gitbook/assets/image%20%2832%29.png)

Wie erwartet bekommen wir eine Fehlermeldung, die besagt, dass der Ort des Bewerbers Dortmund sein muss. Diese Fehlermeldung wird durch die von uns spezifizierte Validierungsregel erzeugt. Wenn wir jetzt im Request den Wert des Attributs Ort von "Berlin" auf "Dortmund" verändern, dann kommt die Fehlermeldung nicht mehr.

![](../.gitbook/assets/image%20%2836%29.png)



## 5. Generierung des Client Proxies

Nun läuft der Service und validiert fleißig die Requests. Üblicherweise hat ein Service mehr als nur eine Validierungsregel. Je nach Komplexität des Services bzw. aller Services in einem Microservice-Ökosystem können es weitaus mehr als 1000 Regeln sein. Die Konsumenten bzw. die Clients bekommen die Fehler oft erst vom Server. Dies führt nicht nur zu vergeudeter Zeit, sondern resultiert auch in einer unnötigen Belastung der Server. Besser wäre es, wenn man bereits Client-seitig Daten validieren könnte. Das hat mehrere Vorteile:

1. Laufzeit-Performance
2. Die Server werden entlastet
3. Die Clients können offline entwickelt werden, da das Exceptionhandling zum Teil auf dem Client passiert.
4. Service Provider und Consumer müssen lediglich einen Service Contract definieren und können unabhängig voneinander entwickelt werden.

Der Custom openVALIDATION-OpenAPI Generator ermöglicht die Generierung von Validierungsregeln nicht nur in einem Service Stub, sondern auch in einem REST Client. Den Service Provider wird es somit ermöglicht, die Validierung der Daten als einen festen Bestandteil des Service Contracts bereitzustellen. Und die Konsumenten können mit Hilfe des Generators automatisch einen Client Proxy erzeugen. Sie müssen sich nicht mehr mit der REST/HTTP Infrastruktur und der Validierung der Daten befassen, da das ja ab jetzt ein fester Bestandteil des Client Proxies ist. Wie genau das funktioniert, sehen wir im folgenden Beispiel. 

Zunächst generieren wir einen Client Proxy mit dem folgenden CLI Aufruf:

{% tabs %}
{% tab title="Window" %}
```bash
java -jar ov-openapi-generator-cli.jar generate ^
     -g ov-javascript-client ^
     -i oapi.spec.yaml ^
     -o out/client
```
{% endtab %}

{% tab title="Unix" %}
```bash
java -jar ov-openapi-generator-cli.jar generate \
     -g ov-javascript-client \
     -i oapi.spec.yaml \
     -o out/client
```
{% endtab %}
{% endtabs %}

Der CLI-Aufruf erzeugt einen Client Proxy als ein JavaScript Projekt im Ordner **out/client** unterhalb des aktuellen Arbeitsverzeichnisses. Der Client ist ein Nodejs Projekt für das zuerst die abhängigen Packages installiert werden müssen.

```bash
cd out/client
npm install
```

Wir möchten den ClientProxy direkt in einer HTML Seite einbinden. Damit das möglich ist, müssen wir aus dem JS Projekt eine einzelne, im Browser ausführbare JavaScript Datei erzeugen. Das machen wir mit Hilfe eines kleinen Tools namens [Browserify](http://browserify.org/). Browserify in ein npm Package, welches man global installieren kann:

```bash
npm install -g browserify
```

Nachdem wir Browserify erfolgreich installiert haben, können wir ein lauffähiges JavaScript Bundle erzeugen: 

```bash
browserify src/index.js --standalone Bundle -o clientproxy.js
```

Der Aufruf erzeugt eine JavaScript Datei namens **clientproxy.js**, welche wir direkt im Browser verwenden können. Jetzt müssen wir nur noch unsere Demo HTML Seite erstellen. Dazu erzeugen wir eine Datei namens **test.html** und fügen folgenden Inhalt ein:

{% code title="test.html" %}
```markup
<html>
  <body>

    <input id="message" />
    <div id="error"></div>
    <button onclick="send()">send</button>

    <script src="clientproxy.js"></script>
    <script type="text/javascript">
        var api = new Bundle.DefaultApi();
        api.apiClient.basePath = "http://localhost:8080";

        var send = function() {
            var data = {
              Ort: document.getElementById('message').value
            };

            api.rootPost(data)
               .then(
                 function(result) {
                   document.getElementById('error').innerHTML = 'API called successfully.';
                 },
                 function(error, data) {
                   var errrorMsg = (error.message)? error.message : error.errors[0].error;

                   document.getElementById('error').innerHTML = "<span style=\"color:red\">" + errrorMsg + "</span>";
                 }
               );
        }
    </script>
  </body>
</html>

```
{% endcode %}

Jetzt vergewissern wir uns, dass der REST Service bereits läuft. Wenn nicht, starten wir den Serivce nochmal wie in [Schritt 3](openapi-tutorial.md#3-generieren-des-service-stubs-als-java-spring-boot) beschrieben. Nun öffnen wir die HTML Seite und geben in das Eingabefeld **"Berlin"** ein. Wie erwartet, kommt die Fehlermeldung _"der **Ort** des Bewerbers **MUSS Dortmund** sein"._ Das Spannende daran ist, dass diese Fehlermeldung bereits auf dem Client, noch vor dem Senden des Requests an den Server, erzeugt wird. Das können wir überprüfen, wenn wir uns den Tab **Network** in DeveloperTools vom Chrome anschauen:

![](../.gitbook/assets/image%20%2859%29.png)

Wenn wir jetzt anstatt **"Berlin", "Dortmund"** eingeben, sehen wir, dass der Request an den Server gereicht wird und ein positiver Response mit dem HTTP Status Code 200 zurückkommt:

![](../.gitbook/assets/image%20%2866%29.png)

Dieses Beispiel demonstriert die Möglichkeit der Generierung von Validierungsregeln, sowohl innerhalb eines Service Stub's als auch innerhalb des entsprechenden Client Proxies. Und das noch für unterschiedliche Technologie Stacks.  

{% hint style="info" %}
In einem Enterprise Umfeld mit mehreren 100 Microservices und einer Vielzahl Consumer pro Service kann so schon eine erhebliche Reduzierung des Implementierungsaufwands bewirkt werden. openVALIDATION und openAPI Specification bringen also enorme Ersparnis mit sich, indem sie einen großen Anteil dieser Aufwände automatisieren.
{% endhint %}

