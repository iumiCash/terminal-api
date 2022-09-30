# Pay for service

## Pay for service request API

Pay for service request.

`POST /api/v1/transactions/send-payment/`


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

    `key` *string* **required**
    :    Payment service type.

    `credentials` *object* **required**
    :    Payment details of key service.

    `amount` *decimal* **required**
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

    `status` [*enum*](/transactions/statuses)

    :    Transaction status. Can be one of follows:

         * `Success`
         * `Pending`
         * `Failed`

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
        curl -v -X POST https://terminal-api.iumi.cash/api/v1/transactions/send_payment/ \
        -H "Authorization: Basic <base64 encoded username:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          },
          "amount": "10.00",
          "fees": {
            "internal_fee": "0.10",
            "external_fee": "0.10"
          },
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
                  "value": "8509813 recharged with SBD $10 (Transaction ID: 0927165328159047)"
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
                  "label": "iumiCash Fee",
                  "value": "0.10 $SBD"
                },
                "external_fee": {
                  "label": "Bmobile Fee",
                  "value": "0.10 $SBD"
                },
                "total": {
                  "label": "Total",
                  "value": "10.00 $SBD"
                }
              }
            }
            ```

??? failure "Service is unavailable"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/unexist/ \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
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

!!! warning "Internal Error Case"
    Due to the fact that users have already deposited money and the terminal cannot return them, 
    it is necessary to ensure that payment for the service will be made as soon as possible 
    ([*the ability to check the transaction status*](/transactions/get-transaction-status)). 
    That is, if the service cannot be paid with the current credentials or because of other reasons, 
    then the user will be able to get the money back at the iumiCash trading office when providing a check, 
    that specifies the ID of the unsuccessful transaction.

??? failure "Authentication header not provided"

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

### Statuses

???+ info "Description"

    Possible values:

    * Success
    * Pending
    * Failed

[idempotency]: /idempotency/
