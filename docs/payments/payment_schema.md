# Payment schema request

## Payment schema request API

Payment schema.

`GET /api/v1/payments/services/<key:string>/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Path parameters

???+ info "Path parameters"

    `key` *string*
    :    Payment's `key` identifier.


### Response

???+ success "Response"

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
    
    `schema` [*object*](#schema)
    :    Payment schema.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/bmobile/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
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
            ```

???+ failure "Payment service does not exist"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash hl_lines="1"
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/not_exist/
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See another [possible errors].

        === "Status code"
            `HTTP 404 Not Found`

        === "Response body"
            ```bash
            {
                "error": "not_found",
                "description": "Payment service 'not_exist' not found",
                "field_errors": []
            }
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/bmobile/
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


### Schema

!!! note
    Schema object may have several dynamic fields, where each of them has next structure.

???+ info "Description"

    `<field_name>` [*object*](#field-schema)
    :    Specific field's schema.


### Field schema

???+ info "Description"

    `label` *string* 
    :    Displaying label of this field

    `placeholder` *string* 
    :    Placeholder of the field.

    `defaultValue` *string* 
    :    The value, which inserted to the field by default

    `type` *enum* 
    :    Input type. Can be one of follows:

         * `phone`
         * `amount`
         * `input`
         * `select`

    `keyboardType` *enum* 
    :    Keyboard type. Can be one of follows:

         * `number-pad` - Only numbers
         * `numeric` - Integer field
         * `default` - Plain text
    :   !!! info
             Not acceptable for `type=select`

    `validators` [*object*](#validators)
    :    Validation field instruction. Has dynamic field structure.

    `credentials` *boolean*
    :    Boolean field which indicate this field as credential field.

    `order` *integer*
    :    Position of the field on rendering structure.


### Validators

Validator object has dynamic field structure.

???+ info "Description"

    `less_than` *integer* 
    :    Input value must be less than this value.

    `more_than` *integer* 
    :    Input value must be more than this value.

    `required` *boolean* 
    :    Input value is required.

    `options` *list of [*options*](#options)* 
    :    Used together when `type=select`.

    `included_countries` *list of [*country code*](#country-code)* 
    :    Used together when `type=phone`. Array of included country codes.

### Options

Available options for select field.

### Country code

Country code as 2-letter abbreviation. For example, `SB`.


## Credentials

In [payment schema](#schema) several fields might have `credentials` boolean flag 
which indicates this field as `credentials field`.

This table shows payment service and their `credential` fields.

| Payment service | Credential fields |
| --------------- | ----------------- |
| `Bmobile`       | `phone`           |
| `SATSOL`        | `login`           |
| `Solomon Water` | `meter`           |


### How to identify credential field

For example, for payment service `bmobile` credentials will be `phone`.

```bash hl_lines="20"
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
```

[possible errors]: ../responses.md#failed-requests
