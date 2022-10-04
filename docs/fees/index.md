# Fees - Introduction

This section will describe the main interactions with commissions for different types of services.

To get a list of commissions for all types of services, you need send request:

## Fees

`GET /api/v1/fees/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

### Response

???+ success "Response"

    *list of [fees](../fees/fees.md)*
    :    Array of all service fees.

### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/fees/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            [
              {
                "key": "solomon_power",
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
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/fees/
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

[possible errors]: ../responses.md#failed-requests
