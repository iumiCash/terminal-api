# Deposit request

## Deposit request API

Deposit money request.

`POST /v1/terminal-api/transactions/cash-in`


### Headers

???+ info "Header parameters"

    `RequestId` *string*
    :    Idempotency key for request. See [idempotency] for more information.

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `username` *string* **required**
    :    User's username.

    `amount` [*integer*][cent integer] **required**
    :    Deposit amount.

    `system_transaction_id` *string* **required**
    :    Terminal's internal `transaction_id`.

    `terminal_id` *string* **required**
    :    From which terminal making deposit request.


### Response

???+ success "Response"

    `transaction_id` *[ID][identifier]* **unique**
    :    iumiCash transaction identifier. You can use it later to check transaction details.

    `system_transaction_id` *string* **unique**
    :    Terminal's internal transaction_id.

    `created_at` *datetime*
    :    Created time of the transaction in ISO format.

    `cheque_content` *list of [*cheque field*](#cheque-field)*
    :    Cheque fields that rendered by terminal.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/cash-in \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": 1000,
          "system_transaction_id": "123456",
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        === "Status code"
            `HTTP 201 Created`

        === "Response body"
            ```json
            {
              "transaction_id": "390IDFE2",
              "system_transaction_id": "123456",
              "created_at": "2022-05-23T12:36:23",
              "cheque_content": [
                {
                  "label": "Created At",
                  "value": "23 May 2022 at 12:36",
                  "position": 1,
                  "section": "main"
                },
                {
                  "label": "Description",
                  "value": "Cash In for @fshevchenko account",
                  "position": 2,
                  "section": "main"
                },
                {
                  "label": "iumiCash Transaction",
                  "value": "390IDFE2",
                  "position": 3,
                  "section": "main"
                },
                {
                  "label": "Terminal transaction",
                  "value": "123456",
                  "position": 4,
                  "section": "main"
                },
                {
                  "label": "Terminal",
                  "value": "190AB",
                  "position": 5,
                  "section": "main"
                },
                {
                  "label": "iumiCash fee",
                  "value": "0.10 $SBD",
                  "position": 6,
                  "section": "total"
                },
                {
                  "label": "Amount",
                  "value": "9.90 $SBD",
                  "position": 7,
                  "section": "total"
                },
                {
                  "label": "Total",
                  "value": "10.00 $SBD",
                  "position": 8,
                  "section": "total"
                }
              ]
            }
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/transactions/cash-in \
        -H "Content-Type: application/json" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": 1000,
          "system_transaction_id": "123456",
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See more [possible errors].

        === "Status code"
            `HTTP 401 Unauthorized`

        === "Response body"
            ```json
            {
                "message": "unauthorized"
            }
            ```


## Objects

Request and response objects

### Cheque field

This schema is used for render specific field on cheque.

???+ info "Description"

    `label` *string*
    :    Label of the field that will be rendered on the cheque.

    `value` *string*
    :    Value of the field that will be rendered on the cheque.

    `position` *integer*
    :    Position of the field on cheque.

    `section` *enum*
    :    Section of the field. This value shows specific section of field. Can be one of follows:
        
        * `main`
        * `total`


### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`


[idempotency]: ../idempotency.md
[possible errors]: ../responses.md#failed-requests
[cent integer]: ../types.md#cent-integer
[identifier]: ../types.md#iumicash-identifier
