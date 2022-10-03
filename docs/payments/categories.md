# Payment categories

## Payment categories API

Get payment categories request.

`GET /api/v1/payments/categories/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Response

???+ success "Response"

    *list of [*payment category*](#payment-category)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/categories/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
            [
              {
                "id": "7552c1f3-dbd2-489c-b998-ec23d0ccc0c7",
                "name": "Internet",
                "icon": "wifi/Feather",
                "order": 1
              }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/categories/
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See another [possible errors].

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


### Payment category

???+ info "Description"

    `id` *UUID* **unique**
    :    Payment category's unique identifier.

    `name` *string* 
    :    Category name.

    `icon` *string* 
    :    Category icon.

    `order` *integer*
    :    Posiiton of category.

[possible errors]: ../responses.md#failed-requests
