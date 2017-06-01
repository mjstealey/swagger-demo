+++
date = "2017-06-01T16:14:11-04:00"
description = "Python Client as generated from swagger"
title = "Python Client"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "pythonclient"
    parent = "exposures"
    weight = 12

+++

Using the generated python-client code from swagger, we ran the code as suggested in the accompanied documentation.

```console
$ cd /PATH/TO/python-client-generated

$ virtualenv -p /usr/local/bin/python3 venv3
Running virtualenv with interpreter /usr/local/bin/python3
Using base prefix '/usr/local/Cellar/python3/3.6.0_1/Frameworks/Python.framework/Versions/3.6'
New python executable in /Users/stealey/Github/irods/swagger-demo/python-client-generated/venv3/bin/python3.6
Also creating executable in /Users/stealey/Github/irods/swagger-demo/python-client-generated/venv3/bin/python
Installing setuptools, pip, wheel...done.

$ source venv3/bin/activate

stealey at dhcp152-54-9-79 in ~/Github/irods/swagger-demo/python-client-generated (master●●)
(venv3)$ pip install -r requirements.txt
Collecting certifi>=14.05.14 (from -r requirements.txt (line 1))
  Using cached certifi-2017.4.17-py2.py3-none-any.whl
Collecting six==1.8.0 (from -r requirements.txt (line 2))
  Using cached six-1.8.0-py2.py3-none-any.whl
Collecting python_dateutil>=2.5.3 (from -r requirements.txt (line 3))
  Using cached python_dateutil-2.6.0-py2.py3-none-any.whl
Requirement already satisfied: setuptools>=21.0.0 in ./venv3/lib/python3.6/site-packages (from -r requirements.txt (line 4))
Collecting urllib3>=1.15.1 (from -r requirements.txt (line 5))
  Using cached urllib3-1.21.1-py2.py3-none-any.whl
Installing collected packages: certifi, six, python-dateutil, urllib3
Successfully installed certifi-2017.4.17 python-dateutil-2.6.0 six-1.8.0 urllib3-1.21.1
```

Package dependencies at this point are (`$ pip freeze`):

```
certifi==2017.4.17
python-dateutil==2.6.0
six==1.8.0
urllib3==1.21.1
```

Based on the [generated documentation](https://github.com/mjstealey/swagger-demo/tree/master/python-client-generated) we made a test file named `exp-test.py` for the "GET exposures" call.

```python
# exp-test.py
import time
import swagger_client
from swagger_client.rest import ApiException
from pprint import pprint

# create an instance of the API class
api_instance = swagger_client.DefaultApi()

try:
    # Get list of exposure types
    api_response = api_instance.exposures_get()
    pprint(api_response)
except ApiException as e:
    print("Exception when calling DefaultApi->exposures_get: %s\n" % e)
```

And ran it:

```console
(venv3)$ python exp-test.py
[{'description': 'exposure to airborne particulates: scores range from 1 (low < '
                '4.0 μg/m3) to 5 (high > 11.37 μg/m3);values returned in μg/m3 '
                'abbreviated as ugm3',
 'exposure_type': 'pm25',
 'has_scores': True,
 'has_values': True,
 'schema_version': '1.0.0',
 'units': 'ugm3'},
 {'description': 'exposure to ozone: scores range from 1 (low ≤ 0.050 ppm) to 5 '
                '(high > 0.125 ppm); values returned in ppm',
 'exposure_type': 'o3',
 'has_scores': True,
 'has_values': True,
 'schema_version': '1.0.0',
 'units': 'ppm'},
 {'description': 'exposure to hazardous waste by facility type (ActiveDisaster; '
                'ActiveSQG; ActiveLQG; ActiveTreatment; InactiveHazard; '
                'Superfund): scores range from 1 (low - residence >50 miles '
                'from site) to 3 (high - residence <1 mile from site); values '
                'returned in miles',
 'exposure_type': 'haz_waste',
 'has_scores': False,
 'has_values': False,
 'schema_version': '1.0.0',
 'units': 'miles'},
 {'description': 'exposure to crime and violence: scores range from 1 (less '
                'crime) – 10 (more crime) relative to national distribution; '
                'values returned as counts per 1000 residents',
 'exposure_type': 'crime',
 'has_scores': False,
 'has_values': False,
 'schema_version': '1.0.0',
 'units': 'count'},
 {'description': 'exposure to residential density: scores range from 1 (pop > '
                '50000) to 3 (pop < 2500); values returned as number of '
                'residents',
 'exposure_type': 'res_den',
 'has_scores': False,
 'has_values': False,
 'schema_version': '1.0.0',
 'units': 'count'},
 {'description': 'exposure to poverty: scores range from 0 (above poverty line) '
                'to 1 (≤ poverty line); values returned as boolean 0 or 1 '
                'corresponding to score',
 'exposure_type': 'poverty',
 'has_scores': False,
 'has_values': False,
 'schema_version': '1.0.0',
 'units': 'boolean'},
 {'description': 'exposure to socioeconomic status: scores range from 1 (high '
                '0%-4.9%) to 4 (low ≥\u200920.0%); values returned as '
                'percentage of US Census-tract population living below the '
                'poverty line',
 'exposure_type': 'ses',
 'has_scores': False,
 'has_values': False,
 'schema_version': '1.0.0',
 'units': 'percent'}]
```

This is all well and good, except that the naming of the client package is generically awful as **swagger_client**. This can be changed to be anything, but does default to swagger_client.

### Docker client extraction

If you're working on a system that has [Docker](https://www.docker.com/) installed on it, the call to generate the client code for a specific language with a defined name can be made in a single call using the [swagger-codegen-cli](https://hub.docker.com/r/swaggerapi/swagger-codegen-cli/) image from Dockerhub.

```console
$ docker run --rm -v ${PWD}:/local swaggerapi/swagger-codegen-cli generate \
    -i https://app.swaggerhub.com/apiproxy/schema/file/mjstealey/environmental_exposures_api/1.0.0/swagger.json \
    -l python \
    -o /local/exposures_api \
    -DpackageName=exposures_api

Unable to find image 'swaggerapi/swagger-codegen-cli:latest' locally
latest: Pulling from swaggerapi/swagger-codegen-cli
709515475419: Pull complete
38a1c0aaa6fd: Pull complete
cd134db5e982: Pull complete
65556e756a60: Pull complete
Digest: sha256:f6d1e17752f84e53c8bb2315c243d8fc8dc8fc8bc567de0ffc3f66089a3457df
Status: Downloaded newer image for swaggerapi/swagger-codegen-cli:latest
[main] INFO io.swagger.parser.Swagger20Parser - reading from https://app.swaggerhub.com/apiproxy/schema/file/mjstealey/environmental_exposures_api/1.0.0/swagger.json
[main] WARN io.swagger.codegen.ignore.CodegenIgnoreProcessor - Output directory does not exist, or is inaccessible. No file (.swager-codegen-ignore) will be evaluated.
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/models/coordinate.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/test_coordinate.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/docs/Coordinate.md
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/models/date_range.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/test_date_range.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/docs/DateRange.md
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/models/exposure.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/test_exposure.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/docs/Exposure.md
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/models/exposure_type.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/test_exposure_type.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/docs/ExposureType.md
[main] WARN io.swagger.codegen.DefaultCodegen - Empty operationId found for path: get /exposures. Renamed to auto-generated operationId: exposuresGet
[main] WARN io.swagger.codegen.DefaultCodegen - Empty operationId found for path: get /exposures/{exposure_type}/values. Renamed to auto-generated operationId: exposuresExposure_typeValuesGet
[main] WARN io.swagger.codegen.DefaultCodegen - Empty operationId found for path: get /exposures/{exposure_type}/scores. Renamed to auto-generated operationId: exposuresExposure_typeScoresGet
[main] WARN io.swagger.codegen.DefaultCodegen - Empty operationId found for path: get /exposures/{exposure_type}/coordinates. Renamed to auto-generated operationId: exposuresExposure_typeCoordinatesGet
[main] WARN io.swagger.codegen.DefaultCodegen - Empty operationId found for path: get /exposures/{exposure_type}/dates. Renamed to auto-generated operationId: exposuresExposure_typeDatesGet
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/apis/default_api.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/test_default_api.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/docs/DefaultApi.md
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/README.md
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/setup.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/tox.ini
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test-requirements.txt
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/requirements.txt
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/api_client.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/rest.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/configuration.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/__init__.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/models/__init__.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/exposures_api/apis/__init__.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/test/__init__.py
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/git_push.sh
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/.gitignore
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/.travis.yml
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/.swagger-codegen-ignore
[main] INFO io.swagger.codegen.AbstractGenerator - writing file /local/exposures_api/.swagger-codegen/VERSION
```
This will generate a new directory named `exposures_api` with the same contents as the swagger generated client, but with the **exposures_api** naming in place. All generated documentation will also make use of *exposures_api* instead of *swagger_client*.

```console
$ ls -R exposures_api
README.md             exposures_api         requirements.txt      test                  tox.ini
docs                  git_push.sh           setup.py              test-requirements.txt

exposures_api/docs:
Coordinate.md   DateRange.md    DefaultApi.md   Exposure.md     ExposureType.md

exposures_api/exposures_api:
__init__.py      api_client.py    apis             configuration.py models           rest.py

exposures_api/exposures_api/apis:
__init__.py    default_api.py

exposures_api/exposures_api/models:
__init__.py      coordinate.py    date_range.py    exposure.py      exposure_type.py

exposures_api/test:
__init__.py           test_coordinate.py    test_date_range.py    test_default_api.py   test_exposure.py      test_exposure_type.py
```
