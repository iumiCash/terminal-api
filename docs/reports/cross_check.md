# Cross-Check Report generation

## Generate Cross-Check report request API

Generate Cross-Check report.

`POST /v1/terminal-api/reports/cross_check/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `date_from` *datetime* **required**
    :    Date from which generate report.

    `date_to` *datetime* **required**
    :    Date to which generate report.

    `currency` *enum*
    :    Currency.

    `terminal` *string*
    :    Terminal.

    `min_amount` *integer*
    :    Records where `amount > min_amount`

    `max_amount` *integer*
    :    Records where `amount < max_amount`


### Response

???+ success "Response"

    *list of [*transaction*](#transaction)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/reports/cross_check/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "currency": "SBD"
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            [
                {
                    "transaction_id": "639471e1a7c82be8b86e20fd",
                    "system_transaction_id": "ABC124",
                    "amount": 1000,
                    "currency": "SBD",
                    "description": "Terminal(Term #3) Cash In",
                    "terminal": "Term #3",
                    "status": "Success",
                    "date": "2022-12-10T11:47:45.718Z",
                    "fees": {
                        "amount": 1000,
                        "internal_fee": 0,
                        "external_fee": 0,
                        "internal_cashback": 0,
                        "external_cashback": 0,
                        "total": 1000
                    }
                }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/reports/cross_check/
        -H "Content-Type: application/json" \
        -d ' \
        {
          "currency": "SBD"
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

### Transaction

???+ info "Description"

    `transaction_id` *[ID][identifier]* 
    :    iumiCash transaction identifier. You can use it later to check transaction details.

    `system_transaction_id` *string* 
    :    Terminal's internal transaction_id.

    `amount` [*integer*][cent integer] **required**
    :    Deposit amount.

    `currency` *enum*
    :    Currency.

    `description` *string*
    :    Description of transaction.

    `terminal` *string*
    :    Terminal.

    `status` *enum*
    :    Transaction's status.

    `date` *datetime*
    :    Date of transaction.

    `fees` [*fees*](#fees)*
    :    Fees of transaction.


### Fees

???+ info "Description"

    `amount` [*integer*][cent integer]
    :    Amount.

    `internal_fee` [*integer*][cent integer]
    :    Internal fee.

    `external_fee` [*integer*][cent integer]
    :    External fee.

    `internal_cashback` [*integer*][cent integer]
    :    Internal cashback.

    `external_cashback` [*integer*][cent integer]
    :    External cashback.

    `total` [*integer*][cent integer]
    :    Total.


[idempotency]: ../idempotency.md
[possible errors]: ../responses.md#failed-requests
[cent integer]: ../types.md#cent-integer
[identifier]: ../types.md#iumicash-identifier
