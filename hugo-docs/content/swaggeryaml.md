+++
date = "2017-06-01T15:14:39-04:00"
description = "Environmental Exposures Swagger definition"
title = "Swagger YAML file"

creatordisplayname = "Michael J. Stealey"
creatoremail = "michael.j.stealey@gmail.com"
lastmodifierdisplayname = "Michael J. Stealey"
lastmodifieremail = "michael.j.stealey@gmail.com"

[menu]

  [menu.main]
    identifier = "yaml"
    parent = "exposures"
    weight = 10

+++

Environmental Exposures API Swagger specification

```yaml
# Data Translator Environmental Exposure API
#
# This API provides access to environmental exposures. An exposure is
# any factor outside a human body that can act upon the body to cause
# a health impact, including chemical entities, biological entities,
# physical factors (e.g., sunlight, temperature), and social environmental
# factors (e.g., crime-induced stress, poverty).
#
# Exposures are parameterized by a range of temporal and spatial
#  factors that determine where a human has been and thus what factors they
#  were exposured to.
#
# Exposures are typically quantified at several levels:
#   - raw value: a measured quantity for the exposure factor typically taken
#       from an environmental sensor or other primary data collection method.
#   - exposure value: a quantity derived from raw values that computes a
#       a net exposure to the factor for a range of spatial and temporal
#       coordinates. This value is often the result of interpolating raw values
#       to cover all relevant spatial temporal coordinates. Values may also
#       combine different raw value sources, such as primary and secondary
#       measures of air particulates. Requests for exposure values may
#       require indicating the computational method and parameters.
#   - exposure score: a quantity derived from exposure and/or raw values that
#       relates the exposure value to a risk score for a particular disease. Thus
#       an exposure score for asthma may be different than a score for alzheimers
#       despite being based on the same exposure values.


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

  /exposures/{exposure_type}/values:
    parameters:
      - $ref: '#/parameters/exposure_type'
    get:
      summary: "Get exposure value for a given environmental factor at exposure location(s)"
      description: "Retrieve the computed exposure value(s) for a given environmental exposure factor, time period, and location(s)."
      produces:
        - application/json
      parameters:
        - $ref: '#/parameters/start_date'
        - $ref: '#/parameters/end_date'
        - $ref: '#/parameters/exposure_point'
        - $ref: '#/parameters/temporal_resolution'
        - $ref: '#/parameters/statistical_type'
        - $ref: '#/parameters/radius'
        - $ref: '#/parameters/page'
      responses:
        200:
          description: OK
          schema:
            type: array
            items:
              $ref: '#/definitions/exposure'
        400:
          description: "Invalid exposure parameter"
        404:
          description: "Values not found for exposure type"

  /exposures/{exposure_type}/scores:
    parameters:
      - $ref: '#/parameters/exposure_type'
    get:
      summary: "Get exposure score for a given environmental factor at exposure location(s)"
      description: "Retrieve the computed exposure score(s) for a given environmental exposure factor, time period, and location(s)."
      produces:
        - application/json
      parameters:
        - $ref: '#/parameters/start_date'
        - $ref: '#/parameters/end_date'
        - $ref: '#/parameters/exposure_point'
        - $ref: '#/parameters/temporal_resolution'
        - $ref: '#/parameters/score_type'
        - $ref: '#/parameters/radius'
        - $ref: '#/parameters/page'
      responses:
        200:
          description: OK
          schema:
            type: array
            items:
              $ref: '#/definitions/exposure'
        400:
          description: "Invalid exposure parameters"
        404:
          description: "Scores not found for exposure type"

  /exposures/{exposure_type}/coordinates:
    parameters:
      - $ref: '#/parameters/exposure_type'
    get:
      summary: "Get exposure location(s) as latitude, longitude coordinates"
      description: "Returns paginated list of available latitude, longitude coordinates for given exposure_type. Optionally the user can provide a latitude, longitude coordinate with a radius in meters to discover if an exposure location is within the requested range."
      produces:
        - application/json
      parameters:
        - $ref: '#/parameters/latitude'
        - $ref: '#/parameters/longitude'
        - $ref: '#/parameters/radius'
        - $ref: '#/parameters/page'
      responses:
        200:
          description: "Exposure points"
          schema:
            type: array
            items:
              $ref: '#/definitions/coordinate'
        404:
          description: "No coordinates found for exposure type"

  /exposures/{exposure_type}/dates:
    parameters:
      - $ref: '#/parameters/exposure_type'
    get:
      summary: "Get exposure start date and end date range for exposure type"
      description: "Returns exposure start date and end date range for given exposure type"
      produces:
        - application/json
      responses:
        200:
          description: "Date range"
          schema:
            $ref: '#/definitions/date_range'
        404:
          description: "No date range found for exposure type"


definitions:
  exposure:
    type: object
    required:
      - exposure_type
    properties:
      exposure_type:
        type: string
        example: "pm25"
      start_time:
        type: string
        format: date-time
        example: "2010-01-15T00:00:00Z"
      end_time:
        type: string
        format: date-time
        example: "2010-01-15T23:00:00Z"
      value:
        type: string
        example: "5.0 |OR| 17.7199974060059"
      units:
        type: string
        example: "7dayrisk |OR| ugm3"
      latitude:
        type: string
        format: float
        example: "35.7795897"
      longitude:
        type: string
        format: float
        example: "-78.6381787"

  exposure_type:
    type: object
    properties:
      exposure_type:
        type: string
        example: "pm25"
      description:
        type: string
        example: "exposure to airborne particulates: scores range from 1 (low < 4.0 μg/m3) to 5 (high > 11.37 μg/m3); values returned in μg/m3 abbreviated as ugm3"
      units:
        type: string
        example: "ugm3"
      has_values:
        type: boolean
        example: true
      has_scores:
        type: boolean
        example: true
      schema_version:
        type: string
        example: "1.0.0"

  coordinate:
    type: object
    properties:
      latitude:
        type: string
        format: float
        example: "35.7795897"
      longitude:
        type: string
        format: float
        example: "-78.6381787"

  date_range:
    type: object
    properties:
      start_date:
        type: string
        format: date-time
        example: "2010-01-01T00:00:00Z"
      end_date:
        type: string
        format: date-time
        example: "2010-01-31T00:00:00Z"


parameters:
  exposure_type:
    name: exposure_type
    in: path
    required: true
    description: "The name of the exposure type (currently limited to pm25, o3, haz_waste, crime, res_den, poverty, ses)."
    type: string
    default: "pm25"

  start_date:
    name: start_date
    in: query
    required: true
    description: "The starting date to obtain exposures for (example 2010-01-06 is January 6th 2010)."
    type: string
    format: date
    default: "2010-01-06"

  end_date:
    name: end_date
    in: query
    required: true
    description: "The ending date to obtain exposures for (example 2010-01-15 is January 15th 2010)."
    type: string
    format: date
    default: "2010-01-15"

  temporal_resolution:
    name: temporal_resolution
    in: query
    required: false
    description: "The temporal resolution to use for results, should be one of 'hour' or 'day'. Default is 'day'"
    type: string
    default: 'day'

  score_type:
    name: score_type
    in: query
    required: false
    description: "The exposure score type to return. The accepted values vary by exposure type. For pm25 values are '7dayrisk', '14dayrisk'. Default is '7dayrisk' (NOT COMPLETE)."
    type: string
    default: '7dayrisk'

  statistical_type:
    name: statistical_type
    in: query
    required: false
    description: "The statistic to use for results, should be one of 'max', 'mean', or 'median'. Default is 'max'"
    type: string
    default: 'max'

  exposure_point:
    name: exposure_point
    in: query
    required: true
    description: "A description of the location(s) to retrieve the exposure for. Locaton may be a single geocoordinate (example '35.9131996,-79.0558445') or a semicomma separated list of geocoord:dayhours giving the start and ending hours on specific days of the week at that location (example '35.9131996,-79.0558445,Sa0813;35.7795897,-78.6381787,other') indicates Saturdays from 8am to 1pm is at one location and all other times are at another location. Hours should be in 24 hours time using 2 digits, days of the week should be the first two characters of the day.If the day of the week does not appear then the time periods apply to all days (example '35.9131996,-79.0558445,0614,35.7795897,-78.6381787,1424') gives two time periods for all days. If hours do not appear then the time period applies to all hours of the day (example '35.9131996,-79.0558445,Sa,35.7795897,-78.6381787,Su')."
    type: string
    default: "35.9131996,-79.0558445"

  latitude:
    name: latitude
    in: query
    required: false
    description: "Search coordinates that match or are like 'latitude'"
    type: string
    default: ""

  longitude:
    name: longitude
    in: query
    required: false
    description: "Search coordinates that match or are like 'longitude'"
    type: string
    default: ""

  radius:
    name: radius
    in: query
    required: false
    description: radius in meters to search within for exposure point
      coordinate is provided. Range from 0 to 500
    type: string
    default: "0"

  page:
    name: page
    in: query
    required: false
    description: "Page number. Return up to 100 items per page"
    type: string
    default: 1
```
