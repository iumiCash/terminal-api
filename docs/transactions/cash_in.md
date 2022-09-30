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

    `system_transaction_id` *string* **required**
    :    Terminal's internal `transaction_id`.

    `terminal_id` *UUID* **required**
    :    From which terminal making deposit request.

    `fees` [*object*](#fees) **required**
    :    Included fees.


### Response

???+ success "Response"

    `transaction_id` *UUID* **unique**
    :    iumiCash transaction identifier. You can use it later to check transaction details.

    `system_transaction_id` *string* **unique**
    :    Terminal's internal transaction_id.

    `created_at` *datetime*
    :    Created time of the transaction in ISO format.

    `cheque_content` [*object*](#cheque-content)
    :    Cheque content can be one of follows:
    
         * `created_at`
         * `description`
         * `system_transaction_id`
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
        curl -v -X POST https://terminal-api.iumi.cash/api/v1/transactions/cash_in/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded username:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": "10.00",
          "system_transaction_id": "31212",
          "terminal_id": "190AB",
          "fees": {
            "internal_fee": 120,
            "external_fee": 100,
            "internal_cashback": 0,
            "external_cashback": 10
          }
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        === "Status code"
            `HTTP 201 Created`

        === "Response body"
            ```bash
            {
              "transaction_id": "390IDFE2",
              "system_transaction_id": "31212",
              "created_at": "2022-05-23T12:36:23",
              "cheque_content": {
                "created_at": {
                  "label": "Created At",
                  "value": "23 May 2022 at 12:36"
                },
                "description": {
                  "label": "Description",
                  "value": "Cash In for @fshevchenko account"
                },
                "transaction_id": {
                  "label": "iumiCash Transaction",
                  "value": "390IDFE2"
                },
                "system_transaction_id": {
                  "label": "Terminal transaction",
                  "value": "390IDFE2"
                },
                "terminal_id": {
                  "label": "Terminal",
                  "value": "190AB"
                },
                "internal_fee": {
                  "label": "Fee 1%",
                  "value": "0.10 $SBD"
                },
                "total": {
                  "label": "Total",
                  "value": "10.00 $SBD"
                }
              }
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
