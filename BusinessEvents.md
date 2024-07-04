# Business Events Proposal

## Intended Audience

- Architects
- Solution Developer

## Summary

The purpose of this document is to give a generic business event schema proposal and a specific MDM product business
events..... *elaborate further*

## Proposal description

### Generic Business Events Definition

| **Element name** | **Element description**                                                                      |
|------------------|----------------------------------------------------------------------------------------------|
| version          | Indicates  the version of the event schema                                                   |
| type             | Specifies the type od the event that was generated as a result of a business action          |
| id               | A unique identifier for the event instance.                                                  |
| correlation id   | An identifier used to correlate the event with other related events r processes.             |
| aggregate        | Represents the type of business object that the event pertains to.                           |
| source           | Identifies the source of the event, such as a service, component or module that produced it. |
| created at       | The Unix timestamp when the event was created                                                |
| occurred in      | Location where the event took place.                                                         |
| payload          | An object containing the data associated with the event.                                     |

#### JSON Schema 

``` json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "BusinessEvent",
  "type": "object",
  "properties": {
    "version": {
      "type": "string",
      "description": "Indicates the version of the event schema."
    }
    "type": {
      "type": "string",
      "description": "Specifies the type of event that was generated as a result of a business action."
    },
    "id": {
      "type": "string",
      "description": "A unique identifier for the event instance."
    },
    "correlation-id": {
      "type": "string",
      "description": "An identifier used to correlate this event with other related events or processes."
    },
    "aggregate": {
      "type": "string",
      "description": "Represents the type of business object that the event pertains to."
    },
    "source": {
      "type": "string",
      "description": "Identifies the source of the event, such as the service, component, or module that produced it."
    },
    "created-at": {
      "type": "integer",
      "description": "The Unix timestamp when the event was created."
    },
    "occurred-at": {
      "type": "integer",
      "description": "The Unix timestamp when the business action that triggered the event actually occurred."
    },
    "occured-in": {
      "type": "object",
      "properties": {
        "latitude": {
          "type": "number"
        },
        "longitude": {
          "type": "number"
        }
      },
      "description": "Location where the event took place."
    },
    "payload": {
      "type": "object",
      "description": "An object containing the data associated with the event.",
      "properties": {
        // Add properties specific to the payload here
      },
      "additionalProperties": true
    }
  },
  "required": [
    "type", "id", "correlation-id", "aggregate", "source", 
    "created-at", "occurred-at", "payload", "version", "occured-in"
  ]
}
```

#### JSON Sample

``` json
{
    "version": "1.0",
    "type": "paycheck-generated",
    "id": "0f778be1-b88f-4e6a-9d68-dde0a9871172",
    "correlation-id": "0188f26b-1e21-4415-acb0-1f7fc9cb643d",
    "aggregate": "employee",
    "source": "9098N",
    "created-at": "1720119590",
    "occurred-at": "1720119590",
    "occurred-in": {
        "latitude": 59.334591,
        "longitude": 18.063240,
    },
    "payload": {
        "employee-id": "S5657F",
        "paycheck-id": "20240704",
        "link": "/employee/S5657F/payckeck/20240704",
    }
}
```

### Product Business Events Definition

![Product definition process](/process.png)

#### Event types (proposal 1)

- product-draft-created (or product-proposal-created)
- product-sent-for-approval (or product-proposal-submitted or product-submitted-for-approval)
- product-released
- product-rejected
- product-term-and-condition-modified

Others?
- *product-discontinuation-announced* -> no longer available lfs
- *product-retired*, *product-sunsetted* -> obsolete lfc
- *product-marketing-started* -> not a status
- *product-discount-available* -> ?

**product-released event sample**

``` json
{
    "version": "1.0",
    "type": "product-released",
    "id": "0f778be1-b88f-4e6a-9d68-dde0a9871172",
    "correlation-id": "0188f26b-1e21-4415-acb0-1f7fc9cb643d",
    "aggregate": "product",
    "source": "9098N",
    "created-at": "1720119590",
    "occurred-at": "1720119590",
    "occurred-in": {
        "latitude": 59.334591,
        "longitude": 18.063240,
    },
    "payload": {
        "id": "ACG12",
        "name": "Green deposit account",
        "type": "Account",
        "link": "/marketed-products/ACG12",
    }
}
```

**product-draft-created event sample**

``` json
{
    "version": "1.0",
    "type": "product-released",
    "id": "0f778be1-b88f-4e6a-9d68-dde0a9871172",
    "correlation-id": "0188f26b-1e21-4415-acb0-1f7fc9cb643d",
    "aggregate": "product",
    "source": "9098N",
    "created-at": "1720119590",
    "occurred-at": "1720119590",
    "occurred-in": {
        "latitude": 59.334591,
        "longitude": 18.063240,
    },
    "payload": {
        "draft-id": "ACG12",
        "name": "Green deposit account",
        "type": "Account",
        "created-by": "person or area"
    }
}
```

#### Event types (proposal 2)

- lifecycle-changed
- term-and-condition-modified

**product-released event sample**

``` json
{
    "version": "1.0",
    "type": "lifecycle-changed",
    "id": "0f778be1-b88f-4e6a-9d68-dde0a9871172",
    "correlation-id": "0188f26b-1e21-4415-acb0-1f7fc9cb643d",
    "aggregate": "product",
    "source": "9098N",
    "created-at": "1720119590",
    "occurred-at": "1720119590",
    "occurred-in": {
        "latitude": 59.334591,
        "longitude": 18.063240,
    },
    "payload": {
        "id": "ACG12",
        "status": "available",
        "link": "/marketed-products/ACG12",
    }
}
```
