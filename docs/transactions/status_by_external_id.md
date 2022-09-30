# Transaction status

## Get transaction status request API

Get transaction status request.

`GET /api/v1/transactions/external/<system_transaction_id>`

!!! tip
    `system_transaction_id` is a terminal system transaction identifier.
    This parameter is used in requests such as 
    [*Cash In*](../transactions/cash_in.md) and [*Pay for service*](../transactions/send_payment.md) requests.

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.

### Response

???+ success "Response"

    `transaction_id` *UUID* **unique**

    :    iumiCash transaction identifier.

    `system_transaction_id` *text*

    :    Terminal system transaction identifier. You can use it later to check transaction details.

    `created_at` *datetime* 

    :    Cheque content can be one of follows:
    
    `status` [*enum*](../transactions/statuses.md) 

    :    Transaction status. Can be one of follows:

         * `Success`
         * `Pending`
         * `Failed`
         * `Unknown`

### Examples

??? example "Registered transaction"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://api.iumi.cash/api/v1/transactions/external/31212 \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded username:password>" \
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        ```bash
        {
          "transaction_id": "390IDFE2",
          "system_transaction_id": "31212",
          "created_at": "2022-05-23T12:36:23",
          "status": "Success"
        }
        ```

!!! warning
    The transaction may not be saved in the iumiCash database for a number of reasons. 
    Therefore, it is important to conduct cross-check transactions.
    Below is an example of a transaction request that exists in the terminal system, but does not exist in iumiCash.

??? example "Unknown transaction"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://api.iumi.cash/api/v1/transactions/external/31213 \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded username:password>" \
        ```

    === "Response"

        A successful request returns the `HTTP 404 Not Found` status code and a JSON response body.

        ```bash
        {
          "transaction_id": null,
          "system_transaction_id": "31213",
          "created_at": null,
          "status": "Unknown"
        }
        ```
