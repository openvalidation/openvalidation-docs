# OpenAPI Codegen

[OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) \(formerly [Swagger](https://swagger.io/)\) and Contract First approach have revolutionized the implementation of REST Services. During the development of **openVALIDATION** we were inspired by their success and approach.

The Contract First approach states that the first step is the Definition of a service contract in a notation that is as human readable as possible. Simultaneously, the notation or specification should be formal to an extend that it is machine readable, so it can be processed in the second step. This basic capability is the foundation of the success of [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) and [OpenAPI Generator](https://openapi-generator.tech/). It has never been easier to build REST or Microservices and the corresponding client proxies. Many Public Web API's from Microsoft Azure, Amazon, Netflix, Deutsche Bank etc. use [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) as well as the corresponding code generators to advance the API Economy. 



## What's the catch?

In such a service contract, rudimentary validation rules are defined in addition to the service operations and the corresponding schemata. Thus, certain attributes of the schema can be defined as mandatory fields. With the help of regular expressions, for example, the format of the e-mail address can be validated automatically. 

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
                name:
                  type: string
                mail:
                  type: number
                  pattern: '^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$'
              required:
                - name
      responses:
        '200':
          description: ok

```

There are also some other dependency checks. 

The validation integrated in OpenAPI Specification is based on the [JSON Schema Validation](https://json-schema.org/latest/json-schema-validation.html)  specification. Each of the specified validation options mainly address the validation of the data structure and the format of an attribute. More complex validation rules with nested conditions and various comparison operations that check the actual content of the data are not included in the scope of JSON Schema Validation. Nevertheless, complex validation rules are an integral part of the services or service oriented architecture.

The problem is that there is no way to declare such complex rules in OpenAPI Specification. The problem is even bigger: There is no way to describe validation rules in any standardized formal nomenclature. The only option that remains is to describe the rules either in a programming language or in prose, which is then manually implemented by a developer in his technology stack.

{% hint style="warning" %}
There is still no way to describe complex validation rules formally enough to automatically generate program code in different programming languages.
{% endhint %}



## openVALIDATION provides the solution

We believe it is an open gap in the OpenAPI specification!

The exact gap that openVALIDATION closes. The rules written in the grammar of openVALIDATION are formal enough to be part of a formal specification. They are easy to define and ideally suited for the Contract First approach. At the same time, they can be processed automatically. These capabilities have allowed us to extend the standard functionality of OpenAPI Specification and the OpenAPI Generator. So that from now on it is possible to define the validation rules in the service contract in order to generate complete service stubs and client proxies including validation rules.

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
                name:
                  type: string
                mail:
                  type: number
            x-ov-rules:
              culture: en
              rule: |  
                The name HAS to be Alex
                AND Email MUST CONTAIN @yahoo.com
      responses:
        '200':
          description: ok
```

openVALIDATION comes with its own OpenAPI extension, which defines the corresponding validation rules for the respective service operation. In the own OV-OpenAPI Generator this extension is processed automatically, so that program code with the corresponding validation rules is generated from it. This code is inserted into the standard generation process of OpenAPI Generator. At the end a Java Spring Service Stub with implemented validation rules is created.

![Custom openVALIDATION-OpenAPI Generator allows generation of validation rules.](../.gitbook/assets/image%20%2815%29.png)



{% hint style="info" %}
The nice thing about OpenAPI and vendor extensions is that they are simply ignored by the standard OpenAPI generator. I.e. you could use openVALIDATION Rules for documentation purposes only. With the bonus that if you use the Custom OV-OpenAPI Generator, the validation rules can be created automatically.
{% endhint %}





