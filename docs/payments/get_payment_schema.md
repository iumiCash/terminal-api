# Payment schema request

## Payment schema request API

Deposit money request.

`GET /api/v1/payments/schemas/<key:string>/`

!!! tip
    `key` query parameter is payment type. See [Payment types](../payments/get_actual_payments.md) for more details.

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


???+ success "Response"

    `key` *string* **unique**
    :    Payment type identifier

    `schema` [*object*](#schema)
    :    Payment type schema implementation object. Can contain the next fields:

         * `<credential>` (*variable*)
         * `amount` (*required*)
         * `description` (*not required*)


## Schema



## Credentials

