# Get Cash in details

## Get cash_in details request API

Get cash_in details.

`GET /v1/terminal-api/cash_in/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`


### Response

???+ success "Response"

    `status` [*object*](#cash-in-status)
    :    Cash_in status

    `fee` [*object*](#fee)
    :    Cash_in fee.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/cash_in/ \
        -H "Authorization: Basic <base64 encoded email:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            {
              "status": {
                "status": "Active",
                "available": true,
                "color": ""
              },
              "fee": {
                "key": "internal",
                "max_amount": 1000000,
                "rules": [
                  {
                    "less_than": 1000100,
                    "more_than": 0,
                    "external_fee_in_percents": 0,
                    "internal_fee_in_percents": 0,
                    "external_fee_fix": 0,
                    "internal_fee_fix": 0,
                    "external_cashback_in_percents": 0,
                    "internal_cashback_in_percents": 0,
                    "external_cashback_fix": 0,
                    "internal_cashback_fix": 0
                  }
                ]
              }
            }

            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/users/my_username/
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

### Cash in status

???+ info "Description"

    `status` *string*
    :    Status name.
        
    `available` *boolean*
    :    Is cash_in available.

    `color` *string*
    :    Status color.

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


[possible errors]: ../responses.md#failed-requests
[cent integer]: ../types.md#cent-integer
