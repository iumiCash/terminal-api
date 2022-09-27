# Retrieve user

## Retrieve user API

Retrieve user by `username`

`GET /api/v1/users/<username:str>/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Response

???+ success "Response"

    `username` *string* **unique**
    :    User's username.

    `first_name` *string*
    :    User's first name.

    `last_name` *string*
    :    User's last name.

    `currency_code` *enum*. See [available currencies](#currencies).
    :    Currency of account.

### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/john_doe/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 200 OK` status code and a JSON response body.

        === "Status code"
            `HTTP 200 OK`

        === "Response body"
            ```bash
            {
              "username": "john_doe",
              "first_name": "John",
              "last_name": "Doe",
              "currency_code": "NZD"
            }
            ```

???+ failure "Username not exist"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/unexist/ \
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

???+ failure "Authentication header not provided"

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
