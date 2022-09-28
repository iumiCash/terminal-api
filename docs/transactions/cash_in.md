# Deposit request

## Deposit request API

Deposit money request.

`POST /api/v1/transactions/cash_in/`


### Headers

???+ info "Header parameters"

    `RequestId` *string*
    :    Idempotency key for request. See [idempotency] for more information.

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `username` *string* **required**
    :    User's username.

    `amount` *Decimal* **required**
    :    Deposit amount.

    `terminal_id` *UUID* **required**
    :    From which terminal making deposit request.

    `fees` [*object*](#fees) **required**
    :    Included fees.


### Response

???+ success "Response"

    `transaction_id` *UUID* **unique**

    :    iumiCash transaction identifier. You can use it later to check transaction details.

    `created_at` *datetime*

    :    Created time of the transaction in ISO format.

    `cheque_content` *object* 

    :    Cheque content can be one of follows:
    
         * `created_at`
         * `description`
         * `transaction_id`
         * `terminal_id`
         * `internal_fee`
         * `external_fee`
         * `total`

    :   !!! info
            Each parameter from `cheque_content` has the structure with the next format:
    
            * `label`: Name of the field. Usually displayed on the left side
            * `value`: Formatted value of the field. Usually displayed on the right side

### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://terminal-api.iumi.cash/api/v1/users/john_doe/ \
        -H "Authorization: Basic <base64 encoded username:password>" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": "10.00",
          "internal_fee": "0.10",
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
            {
              "username": "john_doe",
              "first_name": "John",
              "last_name": "Doe",
              "currency_code": "NZD"
            }
            ```

???+ failure "Username not exist"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/unexist/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        When username not exist, you will see error message. 

        !!! Tip
            [Here](#) you can find more possible errors.

        === "Status code"
            `HTTP 404 Not Found`

        === "Response body"
            ```bash
            {
                "error": "not_exist",
                "description": "Username not exist",
                "field_errors": []
            }
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/my_username/
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            [Here](#) you can find more possible errors.

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

### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`

### Fees

???+ info "Description"

    Fees.

[idempotency]: /idempotency/
