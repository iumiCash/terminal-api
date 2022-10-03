# Available payments

## Available payments API

Get available payments request.

`GET /api/v1/payments/payments/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Query parameters

???+ info "Query parameters"

    `category` *string*
    :    You can use this query parameter to filter payments by `category`.


### Response

???+ success "Response"

    *list of [*payment*](#payment)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/payments/ \
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
                "key": "bmobile",
                "description": "Bmobile service",
                "category": "mobile",
                "image_url": "https://example.com/image.jpg"
              },
              {
                "key": "satsol",
                "description": "Satsol service",
                "category": "internet",
                "image_url": "https://example.com/image.jpg"
              },
              {
                "key": "solomon_water",
                "description": "Solomon water service",
                "category": "utilities",
                "image_url": "https://example.com/image.jpg"
              }
            ]
            ```

???+ success "Successful request with query params"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        !!! tip
            You can filter available payments. For example, by `category`.
        
        ```bash hl_lines="1"
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/payments/?category=mobile \
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
                "key": "bmobile",
                "description": "Bmobile service",
                "category": "mobile",
                "image_url": "https://example.com/image.jpg"
              }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/payments/
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


### Payment

???+ info "Description"

    `key` *string* **unique**
    :    Payment's unique identifier.

    `category_id` *UUID* 
    :    Payment's category identifier.

    `category_name` *string* 
    :    Payment's category name.

    `description` *string*
    :    Description of payment.

    `image_url` *URL string*
    :    Payment's image url.

[possible errors]: ../responses.md#failed-requests
