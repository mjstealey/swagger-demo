+++
date = "2017-05-31T15:27:15-04:00"
description = ""
title = "1. What is Swagger?"

[menu]

  [menu.main]
    identifier = "swagger"
    parent = ""
    weight = 1

+++

Swagger allows you to describe the structure of your APIs so that machines can read them.

The ability of APIs to describe their own structure is the root of all awesomeness in Swagger. Why is it so great? Well, by reading your API’s structure, we can automatically build beautiful and interactive API documentation. We can also automatically generate client libraries for your API in many languages and explore other possibilities like automated testing.

Swagger does this by asking your API to return a YAML or JSON that contains a detailed description of your entire API. This file is essentially a resource listing of your API which adheres to [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md). The specification asks you to include information like:

- What are all the operations that your API supports?
- What are your API’s parameters and what does it return?
- Does your API need some authorization?
- And even fun things like terms, contact information and license to use the API.

You can write a Swagger spec for your API manually, or have it generated automatically from annotations in your source code. Check [swagger.io/open-source-integrations](https://swagger.io/open-source-integrations/) for a list of tools that let you generate Swagger from code.

![swagger.io]({{<baseurl>}}/images/swaggerio.png)

**The goal of Swagger** is to define a standard, language-agnostic interface to REST APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined via Swagger, a consumer can understand and interact with the remote service with a minimal amount of implementation logic. Similar to what interfaces have done for lower-level programming, Swagger removes the guesswork in calling the service.

Technically speaking - Swagger is a [formal specification](http://swagger.io/getting-started/specification) surrounded by a large ecosystem of tools, which includes everything from front-end user interfaces, low-level code libraries and commercial API management solutions.

### Swagger Editor

![swagger editor]({{<baseurl>}}/images/swaggereditor.png)

### Why are there two different UIs for Swagger?

Indeed there are two different user interfaces for working with Swagger definitions. The Editor tab provides a YAML-based text editor with contextual auto-suggestions for content. On the right side of the screen, a live rendering is produced, making it easy to visualize the API definition that you’re creating.

Alternatively the UI tab will show you a read only view of the API, tailored for consumers of the API. We have found that these distinct views give the most flexibility for understanding the intent of the API.

### What are Integrations?

Integrations are free add-ons to your API definition on SwaggerHub to improve and expand its functionality. These Integrations help connect your API to a host of 3rd party tools which allow you to go beyond just API design on SwaggerHub! You could sync your definition with a GitHub repository, quickly generate a mock for your Swagger definition or create a webhook to trigger for certain events on SwaggerHub!

[Learn more here](https://app.swaggerhub.com/help/integrations/index).

### What is SwaggerHub for?

SwaggerHub is an integrated API Development platform, built for teams, that brings the core capabilities of the Swagger framework to design, build, document and deploy APIs. SwaggerHub enables development teams to collaborate and coordinate the entire lifecycle of an API with the flexibility to integrate with the toolset of your choice.

![swaggerhub]({{<baseurl>}}/images/swaggerhub.png)

### But I use GitHub / BitBucket / GitLab, how is this different?

Source control is great for source. But API definitions aren’t quite the same - they deserve their own, first-class treatment. SwaggerHub works in conjunction with version control systems, so hunting through source code should no longer be necessary.

SwaggerHub does allow connections to the GitHub, GitLab and Bitbucket, with others on the way. See here to learn more about our integrations.

### What does Publishing an API do?

When you create an API and make it available for users to consume, you are creating a contract for them. They rely on that definition to work a certain way, and breaking changes will potentially break their integrations.

Publishing an API is specific to a single version of an API. You should do so when the API ships and users can rely on the signatures. It tells your teams and consumers that your API is in a stable state. Once published, it is read-only and cannot be changed.

When published, you should consider making changes in a new version of your API.

After you publish, you may want to update the default version of the API. This is what is shown in search results, or when someone navigates to your API directly without a specific version number. You can [learn more about versioning here](https://app.swaggerhub.com/help/apis/versioning).

Of course, there are always unforeseen situations where you may have a typo or need to make an emergency change. You can **Unpublish** your API but please do so carefully. Trust with your users is precious!

[Learn more about publishing APIs here](https://app.swaggerhub.com/help/apis/publishing-api).
