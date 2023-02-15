# Check credentials request

## Check credentials request API

Check credentials request.

`POST /v1/terminal-api/payments/check_credentials`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Request

???+ info "Request body"

    `key` *string* **required**
    :    Payment type identifier.

    `credentials` [*object*](../payments/get.md#credentials)
    :    Payment credentials. Credential data various depending on `key`.

### Response

???+ success "Response"

    `details` *string*
    :    Details about requested credentials. Terminal can display this value for additional credentials check.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/payments/check-credentials/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          }
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            {
              "details": "John Doe"
            }
            ```

???+ failure "Invalid credentials"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/payments/check-credentials/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "00000"
          }
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 404 Not Found`

        === "Response body"
            ```json
            {
              "error": "not_found",
              "description": "Bmobile credentials not found",
              "field_errors": []
            }
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/payments/check-credentials/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          }
        }
        '
        ```

    === "Response"

        If you not provide authentication header, you will see error message.

        !!! Tip
            See another [possible errors].

        === "Status code"
            `HTTP 401 Unauthorized`

        === "Response body"
            ```json
            {
              "message": "unauthorized"
            }
            ```

[possible errors]: ../responses.md#failed-requests
