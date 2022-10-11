# FAQ request

## FAQ request API

Frequently Asked Questions request.

`GET /api/v1/faq/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Response

???+ success "Response"

    *list of [*faq*](#faq)*


### Examples

???+ success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/faq/ \
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
                "question": "How to send requests?",
                "answer": "To send requests you can use any programming language.",
                "category": {
                  "name": "technical",
                  "label": "Technical questions"
                }
              },
              {
                "question": "How to use daily requests?",
                "answer": "You can use daily requests same as usual requests.",
                "category": {
                  "name": "technical",
                  "label": "Technical questions"
                }
              }
            ]
            ```

???+ failure "Authentication header not provided"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/faq/
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


### FAQ

???+ info "Description"

    `question` *string*
    :    Question.

    `answer` *string*
    :    Answer.

    `category` [*object*](#category)
    :    Question category.


### Category

???+ info "Description"

    `name` *string*
    :    Name.

    `label` *string*
    :    Label.


[possible errors]: ../responses.md#failed-requests
