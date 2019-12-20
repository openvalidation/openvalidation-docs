# open\* what?

openVALIDATION enables programming of complex validation rules using natural language, such as German or English. 

![](.gitbook/assets/language-to-code.png)

The rules recorded in natural language are readable not only by humans but also by the computer and therefore no longer need to be programmed by a software developer. This task is now taken over by openVALIDATION. With integrated code generators, corresponding source code in the desired programming language can be generated automatically, such as Java, C\#, JavaScript or Python, with more to come. This code can then be integrated into any application \(services, frontends, middleware\). 

_Write once, DONT CODE and run everywhere!_

\_\_

[![](.gitbook/assets/button1%20%285%29.PNG)](https://downloadarchive.blob.core.windows.net/openvalidation-generator/openvalidation.jar)  
[![](.gitbook/assets/button2%20%283%29.PNG)\_\_](https://downloadarchive.blob.core.windows.net/openvalidation-openapi-generator/ov-openapi-generator-cli.jar) __

\_\_

### Natural grammar

Grammar based on a natural language is both **formal** and **natural**. This distinguishes this grammar from other programming languages or domain-specific languages. It allows the use of additional semantic or grammatical content. The additional content is only relevant for human readability. The machine, on the other hand, ignores this addition. Thus it is possible to express the rules in a grammatically correct way on the one hand and to give them a semantic context on the other. This all makes the rules easier to understand. Rules formulated with openVALIDATION are thus at the same time a formal, machine-processable specification, but also a documentation that is easy for people to understand.

![](.gitbook/assets/grammar2.png)

### What are validation rules?

Every time data is transmitted, it must be checked for correctness. Such a check is also called plausibility check or **validation**. There are usually several checks, one of which is called a **validation rule**.

Validation rules are often required in user interfaces, services, CLI's or at different places in business processes, i.e. everywhere where data is processed. Here is an example that is certainly familiar to everyone:

![](.gitbook/assets/login-2.png)

Two different validation rules are implemented here. The first rule checks the format of the e-mail address and the second rule checks the length of the password. This is certainly a very simple example. With the complexity of the application, the complexity of the validation rules also increases. For example, an online form for the conclusion of a motor vehicle insurance policy can contain well over 1000 validation rules. 



