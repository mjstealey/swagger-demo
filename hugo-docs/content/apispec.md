+++
date = "2017-05-31T15:27:39-04:00"
description = ""
title = "3. API Specification"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "apispec"
    parent = ""
    weight = 3

+++

### The OpenAPI Specification (fka The Swagger Specification)

The goal of The OpenAPI Specification is to define a standard, language-agnostic interface to REST APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined via OpenAPI, a consumer can understand and interact with the remote service with a minimal amount of implementation logic. Similar to what interfaces have done for lower-level programming, OpenAPI removes the guesswork in calling the service.

Use cases for machine-readable API interfaces include interactive documentation, code generation for documentation, client, and server, as well as automated test cases. OpenAPI-enabled APIs expose JSON files that correctly adhere to the OpenAPI Specification, documented in this repository. These files can either be produced and served statically, or be generated dynamically from your application.

Without going into a long history of interfaces to Web Services, this is not the first attempt to do so. We can learn from CORBA, WSDL and WADL. These specifications had good intentions but were limited by proprietary vendor-specific implementations, being bound to a specific programming language, and goals which were too open-ended. In the end, they failed to gain traction.

OpenAPI does not require you to rewrite your existing API. It does not require binding any software to a service--the service being described may not even be yours. It does, however, require the capabilities of the service be described in the structure of the OpenAPI Specification. Not all services can be described by OpenAPI--this specification is not intended to cover every possible use-case of a REST-ful API. OpenAPI does not define a specific development process such as design-first or code-first. It does facilitate either technique by establishing clear interactions with a REST API.

This [GitHub project](https://github.com/OAI/OpenAPI-Specification) is the starting point for OpenAPI. Here you will find the information you need about the OpenAPI Specification, a simple static sample of what it looks like, and some general information regarding the project.

[The pre-release OAS 3.0.0 Specification Branch](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/README.md)

![OpenAPI specification]({{<baseurl>}}/images/openapispec.png)

### smartAPI Specification

This [document](https://websmartapi.github.io/smartapi_specification/) presents a set of 54 metadata elements (organized into five categories) to usefully describe Web-based Application Programming Interfaces (APIs). These elements were developed by the Big Data to Knowledge (BD2K) API Interoperability Working Group, which conducted a survey of API metadata used in the real world. This group developed the smartAPI Specification as an extension of existing repositories such as Programmable Web, Biocatalogue, and available standards including Open API, schema.org, etc. The aim of the BD2K API Interoperability Working Group is to develop a strategy for maximizing interoperability and reuse of Web-based APIs. This specification aims to serve as a standard for API development that will facilitate the efficient communication among APIs and reduce development costs. The smartAPI Specification includes 21 metadata elements beyond those included in the Open API Initiative. The metadata elements are grouped into categories related to APIs, service providers, API operations, operation parameters, and operation responses. For each category, the metadata elements that are mandatory, recommended, or optional are described and illustrated by examples. The widespread adoption of the smartAPI Specification by the community promises to improve the efficiency and lower the costs of API development, promoting cross-API compatibility and resolving current challenges in API usage.

![smartAPI specification]({{<baseurl>}}/images/smartapispec.png)

### Specification enforcement

Specification is enforced within the editor as seen in this example.

- Left: **responsibleOrganization** is left uncommented and violates OpenAPI specification for valid **additionalProperty**
- Right: **responsibleDeveloper** is commented and violates smartAPI specification for "Missing required property: responsibleDeveloper"

![Enforce specification]({{<baseurl>}}/images/enforcespec.png)
