+++
date = "2017-05-31T15:27:25-04:00"
description = ""
title = "2. Swagger vs SmartAPI"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "smartapi"
    parent = ""
    weight = 2

+++

### What is a smartAPI?

The smartAPI project aims to maximize the FAIRness (Findability, Accessibility, Interoperability, and Reusability) of web-based Application Programming Interfaces (APIs). Rich metadata is essential to properly describe your API so that it becomes discoverable, connected, and reusable. We have developed a openAPI-based [specification](https://websmartapi.github.io/smartapi_specification/) for defining the key API metadata elements and value sets. smartAPIs leverage the [Hydra Web API specification](http://www.hydra-cg.com/) and [JSON-LD](http://json-ld.org/) for providing semantically annotated JSON content that can be treated as [Linked Data](http://linkeddata.org/).

![smartAPI-info]({{<baseurl>}}/images/smartapi.png)

### smartAPI Editor: A tool for semantic annotation of Web APIs

smartAPI Editor is an an extension to [Swagger Editor](https://github.com/swagger-api/swagger-editor/releases). Swagger Editor lets you edit your API document in in YAML inside your browser and to preview documentations in real time.

**smartAPI editor:**

- Validates your API document against [smartAPI specifications](https://github.com/WebsmartAPI/swagger-editor/blob/master/node_modules_changes/schema.json), an extended version of openAPI specification.
- Lets you Save your API document into [smartAPI registry](http://smart-api.info/registry/).
- Enhances auto-suggestion functionality for metadata elements by providing the element's conformance level (Required, Recommended, Optional).
- Enhances auto-suggestion functionality for metadata values by suggesting a list of values used by other APIs along with and sorted by their usage frequency.
- Enables semantic annotation of parameters and responses of the API:
  - auto-suggests values for `parameters.parameterValueType` from [identifiers.org](http://identifiers.org/) along with their usage frequency by other APIs.
  - Integrates the editor with [smartAPI profiler](http://smart-api.info/profiler) which automatically annotates the `responses.responseDataType` of the API.

![smartAPI-editor]({{<baseurl>}}/images/smartapieditor.png)

### Why do we need smartAPIs?

Data analysis is increasingly being performed using cloud-based, web-friendly application programming interfaces (APIs). Thousands of tools and APIs are available through web service registries such as [Programmable Web](http://www.programmableweb.com/), [BioCatalogue](https://www.biocatalogue.org/) and cloud platforms such as [Galaxy](https://galaxyproject.org/). Searching these and other API repositories to find a set of tools to retrieve or operate on data of interest presents a number of formidable challenges: users must not only supply the right combination of search terms, but must also closely examine the API outputs to determine whether they can be connected together. SmartAPIs tackle this challenge because they contain the rich metadata needed to precisely describe the service and the data that it operates on or provides.

### What does the smartAPI offer?

Richly annotated APIs will provide the assets by which users will be able to search, browse, query, and reuse APIs. Users and intelligent agents being able to traverse a highly connected network of smartAPIs. Users will be able to automatically find APIs whose outputs are the inputs for other services. Ultimately, we envision the automatic generation of workflows that take users to where they want to go with what they have today.

### Questions / Concerns regarding smartAPI

**NOTE**: The following observations should be taken with a grain of salt as the smartAPI effort is still under development.

Project looks to have recently diverged it's source from the swagger fork it was using: [https://github.com/WebsmartAPI/smartAPI-editor](https://github.com/WebsmartAPI/smartAPI-editor)

- Don't know if this was strictly for [DOI purposes](https://zenodo.org/record/580097#.WTBJLcaZPUI), or for work going forward, but detaching from the original repository makes it difficult to include future improvements made to the swagger source itself.
- The branch from which the DOI version is from shows "**This branch is 54 commits ahead, 190 commits behind swagger-api:master.**" so there is concern about the fork growing stale with respect to the current state of OpenAPI as implemented in swagger.
- The OpenAPI Specification (OAS) 3.0 is in [pre-release](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/README.md) and it's unclear how this would be ported into smartAPI.

It was denoted during that hackathon that parameters were not necessarily parsed as expected when compared to standard swagger.

- This was the case with the [Broad probabilistic graphical models translator](http://smart-api.info/registry/) and brought to Michel's attention.
- Transferred the Broad pgm swagger.yaml file to swagger-editor for testing and syntax passed once smartAPI specific components were commented out.

Seemingly valid syntax from swagger-editor is not always accepted by smartAPI-editor (Exposures API)

- The editor wouldn't flag anything in particular, but the spec would fail when attempting to save to the registry with a 500 error.
- Unsure if smartAPI is updating a version of swagger-codegen to correspond to changes made w.r.t. specifications.
