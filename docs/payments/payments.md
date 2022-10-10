# Available payment services

## Available payments services API

Get available payments services request.

`GET /api/v1/payments/services/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Query parameters

???+ info "Query parameters"

    `category_id` *[ID][identifier]*
    :    You can use this query parameter to filter payments by `category`.


### Response

???+ success "Response"

    *list of [*payment*](#payment)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            [
              {
                "key": "bmobile",
                "description": "Bmobile service",
                "category_id": "8c51cb48-624d-4706-ba6a-2608f389d6a2",
                "category": "Mobile",
                "status": {
                  "status": "Active",
                  "available": true,
                  "color": "#ff0000"
                },
                "image_url": "https://example.com/image.jpg"
              },
              {
                "key": "satsol",
                "description": "Satsol service",
                "category_id": "a9fa2eb9-ee13-4707-aa27-533091b31b88",
                "category": "Internet",
                "status": {
                  "status": "Active",
                  "available": true,
                  "color": "#ff0000"
                },
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
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/?category_id=8c51cb48-624d-4706-ba6a-2608f389d6a2 \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            [
              {
                "key": "bmobile",
                "description": "Bmobile service",
                "category_id": "8c51cb48-624d-4706-ba6a-2608f389d6a2",
                "category": "Mobile",
                "status": {
                  "status": "Active",
                  "available": true,
                  "color": "#ff0000"
                },
                "image_url": "https://example.com/image.jpg"
              }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/payments/services/
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See another [possible errors].

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


### Payment

???+ info "Description"

    `key` *string* **unique**
    :    Payment's unique identifier.

    `description` *string*
    :    Description of payment.

    `category_id` *[ID][identifier]* 
    :    Payment's category identifier.

    `category` *string* 
    :    Payment's category name.

    `status` [*object*](#payment-status)
    :    Payment's status.

    `image_url` *URL string*
    :    Payment's image url.
    

### Payment status

???+ info "Description"

    `status` *string*
    :    Payment service status.
        
    `available` *boolean*
    :    Is payment service available for transactions.

    `color` *string*
    :    Status color.


[possible errors]: ../responses.md#failed-requests
[identifier]: ../types.md#iumicash-identifier
