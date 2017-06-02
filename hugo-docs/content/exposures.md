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

Short walkthrough example of how the Exposures API was implemented using swagger tools in Python.

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

**database**

We had a pre-existing PostgreSQL/PostGIS database that we were making use of, so there was the need to generate the appropriate models for use by our API server.

The initial models.py file was generated using [sqlacodegen](https://pypi.python.org/pypi/sqlacodegen)

Need to add import statement to `sqlacodegen/codegen.py`in order to account for [geoalchemy2](https://geoalchemy-2.readthedocs.io/en/latest/)

```python
from sqlalchemy.types import Boolean, String
import sqlalchemy
from geoalchemy2 import Geometry, Geography # <-- this
```

```console
$ sqlacodegen --outfile models.py postgres://datatrans:somepassword@192.168.56.101:5432/bdtgreen
```

**https**

Even if we weren't starting with formal authentication and authorization, we at minimum wanted to enable https for all of our services. The documentation for how to do this is straight forward enough for PostgreSQL, but when it came to doing this for the **swagger recommended** connexion package, this is what we found...

![Connexion readthedocs]({{<baseurl>}}/images/connexion.png)

After some package reconnaissance we turned up a way to enable https using native [gevent](http://www.gevent.org/gevent.server.html) features which is an option in the connexion package. This involved a couple extra parameters and [monkey patching](https://en.wikipedia.org/wiki/Monkey_patch).

`app.py`:

```python
#!/usr/bin/env python3

import connexion
from configparser import ConfigParser
from flask_cors import CORS
from gevent import monkey     # <-- this
monkey.patch_all()            # <-- this

if __name__ == '__main__':
    parser = ConfigParser()
    parser.read('ini/connexion.ini')
    app = connexion.App(__name__, specification_dir='./swagger/', server=parser.get('connexion', 'server'))
    app.add_api('swagger.yaml', arguments={'title': 'Environmental Exposures API'})
    if not parser.get('connexion', 'certfile') or not parser.get('connexion', 'keyfile'):
        app.run(port=int(parser.get('connexion', 'port')),
                debug=parser.get('connexion', 'debug'))
    else:
        CORS(app.app)
        app.run(port=int(parser.get('connexion', 'port')),
                debug=parser.get('connexion', 'debug'),
                keyfile=parser.get('connexion', 'keyfile'),   # <-- this
                certfile=parser.get('connexion', 'certfile')  # <-- and this
                )
```

As with most things our Exposures API is ever evolving and still requires much optimizing with respect to database queries to be more efficient / timely.

**Docker**

Finally we packaged everything up to be Docker distributable so that deployment could be made rapidly across multiple endpoints as needed to satisfy a reverse proxy load balancer.

The Environmental Exposures API is available in docker as [mjstealey/exposures_api](https://hub.docker.com/r/mjstealey/exposures_api/). There is an assumption that it will be used to connect to a PostgreSQL/PostGIS database pre-populated with the appropriate environmental exposures data tables.

Usage examples will be given in the context of the sample database included in this repository.

Example:

```console
$ docker pull mjstealey/exposures_api
$ docker run -d \
	--name api-server \
	-e POSTGRES_HOST=backend.postgres.server \
	-e POSTGRES_PORT=5432 \
	-e POSTGRES_DATABASE=bdtgreen \
	-e POSTGRES_USERNAME=datatrans \
	-e POSTGRES_PASSWORD=somepassword \
	-p 5000:5000 \
	mjstealey/exposures_api
```

Once completed the exposures_api would be running at [http://localhost:5000/v1/ui/#/](http://localhost:5000/v1/ui/#/)

The **exposure_api** uses environment variables to set itself up, and these can be passed in either individually, or by use of an environment file.

Example environment file (exposures-api.env) with default values

```ini
CONNEXION_SERVER=
CONNEXION_DEBUG=True
API_SERVER_HOST=localhost
API_SERVER_PORT=5000
API_SERVER_KEYFILE=
API_SERVER_CERTFILE=
POSTGRES_HOST=backend
POSTGRES_PORT=5432
POSTGRES_DATABASE=bdtgreen
POSTGRES_USERNAME=datatrans
POSTGRES_PASSWORD=somepassword
POSTGRES_IP=
```

The user can define the `CONNEXION_SERVER` to be **blank**, **gevent**, or **tornado**. In our case we'll use **gevent**.

The user can also deploy using SSL certificates, as would likely be found in a prodution deployment.

Example (exposures-api.env):

```ini
CONNEXION_SERVER=gevent
CONNEXION_DEBUG=False
API_SERVER_HOST=my.api-server.org
API_SERVER_PORT=443
API_SERVER_KEYFILE=/certs/server.key
API_SERVER_CERTFILE=/certs/server.crt
POSTGRES_HOST=backend.postgres.server
POSTGRES_PORT=5432
POSTGRES_DATABASE=bdtgreen
POSTGRES_USERNAME=datatrans
POSTGRES_PASSWORD=somepassword
POSTGRES_IP=
```

```console
$ docker run -d \
	--name api-server \
	--env-file=exposures-api.env \
	-v /home/docker/certs:/certs  \
	-p 443:443 \
	mjstealey/exposures_api
```

Once completed the exposures_api would be running at [https://my.api-server.org/v1/ui/#/](https://my.api-server.org/v1/ui/#/)


### Python Client

![Python client]({{<baseurl>}}/images/pythonclient.png)
