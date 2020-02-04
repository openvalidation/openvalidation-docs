# open\* was?

openVALIDATION ermöglicht das Programmieren komplexer Validierungsregeln mittels natürlicher Sprache, z.B. in Deutsch oder in Englisch.

![](.gitbook/assets/image%20%2813%29.png)

Die in natürlicher Sprache erfassten Regeln sind nicht nur von Menschen, sondern auch von Maschinen lesbar und müssen daher nicht mehr von einem Softwareentwickler programmiert werden. Diese Aufgabe übernimmt ab jetzt openVALIDATION. Mit integrierten Code-Generatoren kann ein entsprechender Code in der gewünschten Programmiersprache wie Java, C\#, JavaScript und in Zukunft vielen weiteren, automatisch erzeugt werden. Diesen Code kann man anschließend in eine beliebige Anwendung \(Services, Frontends, Middleware\) integrieren.

_Write once, DONT CODE and run everywhere_

[![](.gitbook/assets/button1%20%285%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar)

[![](.gitbook/assets/button2%20%283%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-openapi-generator/ov-openapi-generator-cli.jar)



\_\_

### Natürliche Grammatik

Die auf einer natürlichen Sprache basierende Grammatik ist sowohl **formal** als auch **natürlich**. Das unterscheidet diese Grammatik von anderen Programmiersprachen bzw. domänenspezifischen Sprachen. Sie erlaubt es,  einen zusätzlichen semantischen oder grammatikalischen Inhalt zu verwenden. Der zusätzliche Inhalt ist nur für die menschliche Lesbarkeit von Bedeutung. Die Maschine dagegen ignoriert diesen Zusatz. So ist es möglich, einerseits die Regeln grammatikalisch korrekt auszudrücken und andererseits den Regeln einen semantischen Kontext mitzugeben. Das alles macht die Regeln verständlicher. Die mit openVALIDATION formulierten Regeln sind somit gleichzeitig eine formale, maschinell verarbeitbare Spezifikation, und auch eine für die Menschen leicht verständliche Dokumentation.

![](.gitbook/assets/image%20%2816%29.png)

### Was sind Validierungsregeln?

Wenn Daten übertragen werden, müssen diese auf ihre Richtigkeit geprüft werden. So eine Prüfung nennt man auch Plausibilitätsprüfung bzw. **Validierung**. Üblicherweise gibt es mehrere Prüfungen, wobei eine einzelne davon als **Validierungsregel** bezeichnet wird. 

Oft werden Validierungsregeln in User Interfaces, Services, CLI's oder auch an verschiedenen Stellen in Geschäftsprozesses benötigt, also überall dort, wo Daten verarbeitet werden. Hier ein Beispiel, das sicherlich jedem bekannt ist:

![](.gitbook/assets/image%20%2811%29.png)

Hier sind zwei verschiedene Validierungsregeln implementiert. Die erste Regel prüft das Format der E-Mail Adresse. Und die zweite Regel prüft die Länge des Passworts. Dies ist sicherlich ein sehr einfaches Beispiel. Mit der Komplexität der Anwendung steigt auch die Komplexität der Validierungsregeln. So können z.B. in einem Online Formular zum Abschluss einer KFZ Versicherung weit über 1000 Validierungsregeln enthalten sein.   




