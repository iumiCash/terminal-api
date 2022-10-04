# Consolidated Report generation

## Generate Consolidated report request API

Generate Consolidated report.

`POST /api/v1/reports/consolidated/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

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

!!! danger
    WIP

???+ success "Response"

    `transaction_id` *[ID][identifier]* **unique**
    :    iumiCash transaction identifier.

    `system_transaction_id` *string* **unique**
    :    Terminal's internal transaction_id.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://terminal-api.iumi.cash/api/v1/reports/cross_check/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded username:password>" \
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
            {}
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/reports/cross_check/
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



[idempotency]: ../idempotency.md
[possible errors]: ../responses.md#failed-requests
[cent integer]: ../types.md#cent-integer
[identifier]: ../types.md#iumicash-identifier
