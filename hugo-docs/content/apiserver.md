+++
date = "2017-05-31T15:27:47-04:00"
description = ""
title = "4. API Server"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "apiserver"
    parent = ""
    weight = 4

+++

Server generation is still strictly a developer task and swagger isn't providing any particular magic here.

If you already have a server that delivers valid output in accordance with your swagger specification, then you're done with this section and can move on.

If you're starting from scratch and the swagger specification is the first step for your project, then **swagger codegen** may be of interest to you.

**Swagger Codegen**: Remove tedious plumbing and configuration by generating boilerplate server code in over 20 different languages

Server stubs:

- C# (ASP.NET Core, NancyFx), Erlang, Go, Haskell, Java (MSF4J, Spring, Undertow, JAX-RS: CDI, CXF, Inflector, RestEasy), PHP (Lumen, Slim, Silex, Zend Expressive), Python (Flask), NodeJS, Ruby (Sinatra, Rails5), Scala (Finch, Scalatra)

![swagger server]({{<baseurl>}}/images/swaggerserver.png)

Server stub generator [HOWTO](https://github.com/swagger-api/swagger-codegen/wiki/Server-stub-generator-HOWTO)

The server stubs will use the naming conventions as encoded in the specification file and generate the templates required to start encoding the logic for your server.

It should be noted that there is no safeguards against doing nonsensical things and making a mess of your server code.
