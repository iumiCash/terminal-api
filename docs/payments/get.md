# Get payment service

## Get payment service request API

Payment service.

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

    `category_id` *[ID][identifier]* 
    :    Payment's category identifier.

    `category` *string* 
    :    Payment's category name.

    `description` *string*
    :    Description of payment.

    `status` [*object*](#payment-status)
    :    Payment's status.

    `image_url` *URL string*
    :    Payment's image url.
    
    `schema` [*object*](#schema)
    :    Payment schema.

    `fee` [*object*](#fee)
    :    Related fees.


### Examples

??? success "Successful request"

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
            ```json
            {
              "key": "bmobile",
              "description": "Bmobile service",
              "category_id": "8c51cb48-624d-4706-ba6a-2608f389d6a2",
              "category": "Mobile",
              "status": {
                "status": "Active",
                "available": true,
                "color": "#ff0000"
              },
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
              },
              "fee": {
                "key": "bmobile",
                "max_amount": 100000,
                "rules": [
                  {
                    "less_than": 10100,
                    "more_than": 0,
                    "external_fee_in_percents": 0,
                    "internal_fee_in_percents": 0,
                    "external_fee_fix": 125,
                    "internal_fee_fix": 175,
                    "external_cashback_in_percents": 0,
                    "internal_cashback_in_percents": 0,
                    "external_cashback_fix": 0,
                    "internal_cashback_fix": 0
                  },
                  {
                    "less_than": 1000000,
                    "more_than": 10100,
                    "external_fee_in_percents": 0,
                    "internal_fee_in_percents": 0,
                    "external_fee_fix": 125,
                    "internal_fee_fix": 175,
                    "external_cashback_in_percents": 0,
                    "internal_cashback_in_percents": 0,
                    "external_cashback_fix": 0,
                    "internal_cashback_fix": 0
                  }
                ]
              }
            }
            ```

??? failure "Payment service does not exist"

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
            ```json
            {
                "error": "not_found",
                "description": "Payment service 'not_exist' not found",
                "field_errors": []
            }
            ```

??? failure "Authentication header not provided"

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
            ```json
            {
                "error": "unauthotized",
                "description": "Authentication header not provided",
                "field_errors": []
            }
            ```

## Objects

Request and response objects


### Fee

???+ info "Description"

    `key` *string*
    :    Transaction type key

    `max_amount` [*integer*][cent integer]
    :    Transaction maximum amount

    `rules` *list of [*rule*](#rule)*
    :    This field describes which rule element should be applied to the entered amount.
    :   !!! info
            To understand which rule to apply, there are fields `more_than` and `less_than`.
            See [*rule*](#rule) for more details.


### Rule

??? info "Description"

    `less_than` [*integer*][cent integer]
    :   The amount need to be less than or equal the value of this field to accept this rule object.

    `more_than` [*integer*][cent integer]
    :   The amount need to be more than the value of this field to accept this rule object.

    `external_fee_in_percents` *decimal*   
    :    Commission charged by a third party. 
         The commission value is calculated as a percentage of the transaction amount.

     `internal_fee_in_percents` *decimal*   
    :    Commission charged by iumiCash. 
         The commission value is calculated as a percentage of the transaction amount.   
   
     `external_fee_fix` [*integer*][cent integer]
    :    Commission charged by a third party.
         The commission is charged as a fixed value regardless of the transaction amount.

     `internal_fee_fix` [*integer*][cent integer]
    :    Commission charged by iumiCash.
         The commission is charged as a fixed value regardless of the transaction amount.

     `external_cashback_in_percents` *decimal*   
    :   Cashback returned by a third party. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note ""
             This value is negative decimal by default, if exists and not equal 0. 

     `internal_cashback_in_percents` *decimal*   
    :   Cashback returned by iumiCash. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note ""
             This value is negative decimal by default, if exists and not equal 0. 

     `external_cashback_fix` [*integer*][cent integer]
    :   Cashback returned by a third party. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note ""
             * This value is negative decimal by default, if exists and not equal 0. 

     `internal_cashback_fix` [*integer*][cent integer]
    :   Cashback returned by iumiCash. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note ""
             * This value is negative decimal by default, if exists and not equal 0. 


### Payment status

???+ info "Description"

    `status` *string*
    :    Payment service status.
        
    `available` *boolean*
    :    Is payment service available for transactions.

    `color` *string*
    :    Status color.

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

```json hl_lines="2 8 20"
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
[identifier]: ../types.md#iumicash-identifier
[cent integer]: ../types.md#cent-integer
