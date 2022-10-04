# Available payment services

## Available payments services API

Get available payments services request.

`GET /api/v1/payments/services/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Query parameters

???+ info "Query parameters"

    `category_id` *UUID*
    :    You can use this query parameter to filter payments by `category`.


### Response

???+ success "Response"

    *list of [*payment*](#payment)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
            [
              {
                "key": "bmobile",
                "description": "Bmobile service",
                "category_id": "8c51cb48-624d-4706-ba6a-2608f389d6a2",
                "category": "Mobile",
                "image_url": "https://example.com/image.jpg",
                "schema": {
                  "phone": {
                    "label": "Phone number",
                    "placeholder": "",
                    "defaultValue": null,
                    "type": "phone",
                    "keyboardType": "number-pad",
                    "includedCountries": [
                      "SB"
                    ],
                    "validators": {
                      "required": true
                    },
                    "credentials": true,
                    "order": 0
                  },
                  "amount": {
                    "label": "Top Up Amount (SBD)",
                    "placeholder": "",
                    "defaultValue": 0,
                    "type": "amount",
                    "keyboardType": "numeric",
                    "includedCountries": null,
                    "validators": {
                      "required": true,
                      "less_than": 1000,
                      "more_than": 1
                    },
                    "order": 1
                  },
                  "description": {
                    "label": "Description",
                    "placeholder": "Bmobile Top Up",
                    "defaultValue": "Bmobile Top Up",
                    "type": "input",
                    "keyboardType": "default",
                    "includedCountries": null,
                    "validators": {
                      "required": false
                    },
                    "order": 2
                  }
                }
              },
              {
                "key": "satsol",
                "description": "Satsol service",
                "category_id": "a9fa2eb9-ee13-4707-aa27-533091b31b88",
                "category": "Internet",
                "image_url": "https://example.com/image.jpg",
                "schema": {
                  "phone": {
                    "label": "Phone number",
                    "placeholder": "",
                    "defaultValue": null,
                    "type": "phone",
                    "keyboardType": "number-pad",
                    "includedCountries": [
                      "SB"
                    ],
                    "validators": {
                      "required": true
                    },
                    "credentials": true,
                    "order": 0
                  },
                  "amount": {
                    "label": "Top Up Amount (SBD)",
                    "placeholder": "",
                    "defaultValue": 0,
                    "type": "amount",
                    "keyboardType": "numeric",
                    "includedCountries": null,
                    "validators": {
                      "required": true,
                      "less_than": 1000,
                      "more_than": 1
                    },
                    "order": 1
                  },
                  "description": {
                    "label": "Description",
                    "placeholder": "Bmobile Top Up",
                    "defaultValue": "Bmobile Top Up",
                    "type": "input",
                    "keyboardType": "default",
                    "includedCountries": null,
                    "validators": {
                      "required": false
                    },
                    "order": 2
                  }
                }
              }
            ]
            ```

???+ success "Successful request with query params"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        !!! tip
            You can filter available payments. For example, by `category`.
        
        ```bash hl_lines="1"
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/?category_id=8c51cb48-624d-4706-ba6a-2608f389d6a2 \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
            [
              {
                "key": "bmobile",
                "description": "Bmobile service",
                "category_id": "8c51cb48-624d-4706-ba6a-2608f389d6a2",
                "category": "Mobile",
                "image_url": "https://example.com/image.jpg",
                "schema": {
                  "phone": {
                    "label": "Phone number",
                    "placeholder": "",
                    "defaultValue": null,
                    "type": "phone",
                    "keyboardType": "number-pad",
                    "includedCountries": [
                      "SB"
                    ],
                    "validators": {
                      "required": true
                    },
                    "credentials": true,
                    "order": 0
                  },
                  "amount": {
                    "label": "Top Up Amount (SBD)",
                    "placeholder": "",
                    "defaultValue": 0,
                    "type": "amount",
                    "keyboardType": "numeric",
                    "includedCountries": null,
                    "validators": {
                      "required": true,
                      "less_than": 1000,
                      "more_than": 1
                    },
                    "order": 1
                  },
                  "description": {
                    "label": "Description",
                    "placeholder": "Bmobile Top Up",
                    "defaultValue": "Bmobile Top Up",
                    "type": "input",
                    "keyboardType": "default",
                    "includedCountries": null,
                    "validators": {
                      "required": false
                    },
                    "order": 2
                  }
                }
              }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See another [possible errors].

        === "Status code"
            `HTTP 403 Forbidden`

        === "Response body"
            ```bash
            {
                "error": "unauthotized",
                "description": "Authentication header not provided",
                "field_errors": []
            }
            ```

## Objects

Request and response objects


### Payment

???+ info "Description"

    `key` *string* **unique**
    :    Payment's unique identifier.

    `category_id` *UUID* 
    :    Payment's category identifier.

    `category_name` *string* 
    :    Payment's category name.

    `description` *string*
    :    Description of payment.

    `image_url` *URL string*
    :    Payment's image url.
    
    `schema` [*object*][payment schema]
    :    Payment schema. Required to render specific service's fields.


[possible errors]: ../responses.md#failed-requests
[payment schema]: ../payments/payment_schema.md#schema
