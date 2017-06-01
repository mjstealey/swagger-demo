+++
date = "2017-05-31T15:27:56-04:00"
description = ""
title = "5. API Client"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "apiclient"
    parent = ""
    weight = 5

+++

**The swagger codegen project** allows generation of API client libraries (SDK generation), server stubs and documentation automatically given an OpenAPI Spec.

API clients:

- ActionScript, Apex, Bash, C# (.net 2.0, 4.0 or later), C++ (cpprest, Qt5, Tizen), Clojure, Dart, Elixir, Go, Groovy, Haskell, Java (Jersey1.x, Jersey2.x, OkHttp, Retrofit1.x, Retrofit2.x, Feign), Kotlin, Node.js (ES5, ES6, AngularJS with Google Closure Compiler annotations) Objective-C, Perl, PHP, Python, Ruby, Scala, Swift (2.x, 3.x), Typescript (Angular1.x, Angular2.x, Fetch, jQuery, Node)

![swagger client]({{<baseurl>}}/images/swaggerclient.png)

If the server code has adhered to the contract as defined in the specification file, then the client code should "just work".

Client code will interpret responses from the server in accordance to the language specified. As an example, python clients will encode things in single quotes, whereas standard JSON is double quoted.
