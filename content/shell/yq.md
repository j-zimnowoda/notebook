---
title: 'Working with YAML files'
metaTitle: 'yq is command-line YAML processor'
metaDescription: 'yq yaml'
---

# Overview

The YAML format is becoming the standard for describing any kind of configuration parameters.
A common uses case is to extract a given parameter or set of parameters from a YAML file
In this article I am going to present some examples that I am often using at my work.

# Examples

## Get all keys under clouds attribute

Input:

```
clouds:
    aws: {}
    amazon: {}
    digitalOcean: {}
    google: {}
    ovh: {}
```

Command:

```
yq r -j $filePath clouds | jq -r '.|keys[]'
```

Output:

```
amazon
aws
digitalOcean
google
ovh
```

## Remove each filed called `required`

Input:

```
definitions:
  address:
    type: object
    properties:
      street_address:
        type: string
      city:
        type: string
      state:
        type: string
    required:
    - street_address
    - city
    - state
type: object
properties:
  billing_address:
    "$ref": "#/definitions/address"
  shipping_address:
    allOf:
    - "$ref": "#/definitions/address"
    - properties:
        type:
          enum:
          - residential
          - business
      required:
      - type
```

Command:

```
yq d test.yaml '**.required.'
```

```
definitions:
  address:
    type: object
    properties:
      street_address:
        type: string
      city:
        type: string
      state:
        type: string
type: object
properties:
  billing_address:
    "$ref": "#/definitions/address"
  shipping_address:
    allOf:
      - "$ref": "#/definitions/address"
      - properties:
          type:
            enum:
              - residential
              - business
```

References:

- https://mikefarah.gitbook.io/yq/
