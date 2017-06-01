+++
date = "2017-05-31T19:52:22-04:00"
description = "Exposures API Example"
title = "6. Exposures API"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "exposures"
    parent = ""
    weight = 6

+++

Example of how the Exposures API took shape using Swagger.

### Specification in Swagger

At the start we had a notion of the data we wanted to serve, a list of sources to get it from, and a good idea of what this data looked like with some example datasets in a PostgreSQL / PostGIS database. Based on this information a specification file was started prior to the existence of any ReSTful service provider, so going down the rabbit hole of the swagger eco-system seemed a sensible thing to do.

We started with a [specification]({{<baseurl>}}/swaggeryaml) in swagger:

```yaml
swagger: "2.0"
info:
  title: "Environmental Exposures API"
  version: "1.0.0"
  contact:
    name: "Michael J. Stealey"
    email: "stealey@renci.org"
    url: "http://renci.org"
    #responsibleDeveloper: "Michael J. Stealey"
    #responsibleOrganization: "RENCI"
  description: "API for environmental exposure models for NIH Data Translator program"
  termsOfService: "None Available"
host: exposures.renci.org
basePath: /v1
schemes:
 - https
 - http

paths:
  /exposures:
    get:
      summary: "Get list of exposure types"
      description: "Returns a list of all available exposure types"
      produces:
        - application/json
      responses:
        200:
          description: "Exposure types"
          schema:
            type: array
            items:
              $ref: '#/definitions/exposure_type'
        404:
          description: "No exposure types found"
...
```

A couple early versions of the specification did make it into the smartAPI registry, but when copying an updated specification to the [smart-api.info/editor](http://smart-api.info/editor/#/) it yielded no errors, however when the “Save” button is pressed it generates a pop-up error, even though the editor states “All changes saved”.

Reference: `Error"<html><title>500: Internal Server Error</title><body>500: Internal Server Error</body></html>"`

### Python-Flask Server

The swagger-editor offers an option to generate server stubs using swagger-codegen in many different languages. We chose to implement our Exposures API in Python and used the **python-flask** option.

![Python-flask server]({{<baseurl>}}/images/pythonserver.png)

The [resultant code](https://github.com/mjstealey/swagger-demo/tree/master/python-flask-server-generated) gives some suggestion as to how it "should" be implemented and made reference to a [Connexion](https://github.com/zalando/connexion) library on top of Flask.

![Swagger generated server]({{<baseurl>}}/images/generatedserver.png)

And when you run the code as described you get [this]({{<baseurl>}}/pythonserver).

### Python Client

![Python client]({{<baseurl>}}/images/pythonclient.png)
