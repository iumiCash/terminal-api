# Pay for service

## Pay for service request API

Pay for service request.

`POST /v1/terminal-api/transactions/payments`


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

    `key` *string* **required**
    :    Payment service type.

    `credentials` [*object*][credentials] **required**
    :    Payment credentials. See [credentials] for more information.

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

    `cheque_content` *list of [*cheque field*][cheque field]*
    :    Cheque fields that will be rendered.

### Examples

??? success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        !!! tip "Data notice"
            Please, pay attention, that data in this example is not real. Example shows only
            usage strategies.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/payments/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          },
          "amount": 1000,
          "system_transaction_id": "123456",
          "terminal_id": "190AB",
          "fees": {
            "internal_fee": 40,
            "external_fee": 30,
            "internal_cashback": 20,
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
                  "value": "8509813 recharged with SBD $10 (Transaction ID: 0927165328159047)",
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
                  "value": "0.40 $SBD",
                  "position": 6,
                  "section": "total"
                },
                {
                  "label": "Bmobile fee",
                  "value": "0.30 $SBD",
                  "position": 7,
                  "section": "total"
                },
                {
                  "label": "iumiCash cashback",
                  "value": "0.20 $SBD",
                  "position": 8,
                  "section": "total"
                },
                {
                  "label": "Bmobile cashback",
                  "value": "0.10 $SBD",
                  "position": 9,
                  "section": "total"
                },
                {
                  "label": "Amount",
                  "value": "9.60 $SBD",
                  "position": 10,
                  "section": "total"
                },
                {
                  "label": "Total",
                  "value": "10.00 $SBD",
                  "position": 11,
                  "section": "total"
                }
              ]
            ```

??? failure "Service is unavailable"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/payments/ \
        -H "Content-Type: application/json" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -H "Authorization: Basic <base64 encoded email:password>"
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          },
          "amount": 1000,
          "fees": {
            "internal_fee": 10,
            "external_fee": 10,
            "internal_cashback": 10,
            "external_cashback": 10,
          },
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        Payment service unavailable. In this example, `Bmobile`.

        !!! Tip
            See more [possible errors].

        === "Status code"
            `HTTP 400 Bad Request`

        === "Response body"
            ```json
            {
                "error": "payment_service_unavailable",
                "description": "Service Bmobile in unavailable",
                "field_errors": []
            }
            ```

!!! warning "Internal Error Case"
    Due to the fact that users have already deposited money and the terminal cannot return them, 
    it is necessary to ensure that payment for the service will be made as soon as possible 
    ([*the ability to check the transaction status*](../transactions/status_by_external_id.md)). 
    That is, if the service cannot be paid with the current credentials or because of other reasons, 
    then the user will be able to get the money back at the iumiCash trading office when providing a check, 
    that specifies the ID of the unsuccessful transaction.

??? failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/payments/ \
        -H "Content-Type: application/json" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          },
          "amount": 1000,
          "fees": {
            "internal_fee": 10,
            "external_fee": 10,
            "internal_cashback": 10,
            "external_cashback": 10,
          },
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See more [possible errors].

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

### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`

### Fees

???+ info "Description"

    `internal_fee` [*integer*][cent integer]
    :    Internal fee amount.

    `external_fee` [*integer*][cent integer]
    :    External fee amount.

    `internal_cashback` [*integer*][cent integer]
    :    Internal cashback amount.

    `external_cashback` [*integer*][cent integer]
    :    External cashback amount.

### Statuses

???+ info "Description"

    Possible values:

    * Success
    * Pending
    * Failed

[idempotency]: ../idempotency.md
[credentials]: ../payments/get.md#credentials
[possible errors]: ../responses.md#failed-requests
[fee object]: ../transactions/cash_in.md#fees
[cheque field]: ../transactions/cash_in.md#cheque-field
[cent integer]: ../types.md#cent-integer
[identifier]: ../types.md#iumicash-identifier
