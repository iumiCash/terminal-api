# Retrieve user

## Retrieve user API

Retrieve user by `username`

`GET /v1/terminal-api/users/<username:str>/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`


### Response

???+ success "Response"

    `username` *string* **unique**
    :    User's username.

    `first_name` *string*
    :    User's first name.

    `last_name` *string*
    :    User's last name.

    `currency` *enum*. See [available currencies](#currencies).
    :    Currency of account.


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/users/john_doe/ \
        -H "Authorization: Basic <base64 encoded email:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```json
            {
              "username": "john_doe",
              "first_name": "John",
              "last_name": "Doe",
              "currency": "NZD"
            }
            ```

???+ failure "Username not exist"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/users/unexist/ \
        -H "Authorization: Basic <base64 encoded email:password>"
        ```

    === "Response"

        When username not exist, you will see error message. 

        !!! Tip
            See another [possible errors].

        === "Status code"
            `HTTP 404 Not Found`

        === "Response body"
            ```json
            {
                "message": "unexist not found"
            }
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://iumi.cash/v1/terminal-api/users/my_username/
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


## Objects

Request and response objects

### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`


[possible errors]: ../responses.md#failed-requests
