# Check credentials request

## Check credentials request API

Check credentials request.

`POST /api/v1/payments/check-credentials/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `key` *string* **required**
    :    Payment type identifier.

    `credentials` [*object*](../payments/get_payment_schema.md#credentials)
    :    Payment credentials

### Response

???+ success "Response"

    `transaction_id` *UUID* **unique**
    :    iumiCash transaction identifier. You can use it later to check transaction details.

    `system_transaction_id` *string* **unique**
    :    Terminal's internal transaction_id.

    `created_at` *datetime*
    :    Created time of the transaction in ISO format.

    `cheque_content` *list of [*cheque field*](#cheque-field)*
    :    Cheque fields that rendered by terminal.


