# Idempotency

You can make idempotent calls any number of times without concern that the server creates
 or completes an action on a resource more than once. You can retry idempotent calls that 
 fail with network timeouts or HTTP 5xx status codes for as long as the server stores the ID. 
 Idempotency enables you to correlate request payloads with response payloads, eliminate duplicate 
 requests, and retry failed requests or requests with unclear responses.

!!! success
    To enforce idempotency on REST API POST calls, use the `RequestId` request header, 
    which contains a unique user-generated ID that the server stores for a period of time.


!!! warning 
    For example, when you include a previously specified `RequestId` header in a request, 
    iumiCash returns the latest status of the previous request that used that same header. 
    Conversely, when you omit the `RequestId` header from a request, iumiCash duplicates the request.


???+ example "Examples"

    === "Request"

        Example request with cURL.
        
        !!! Tip
            Here were are using `RequestId` idempotency key.

        ```bash hl_lines="4"
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/cash_in/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": 1000,
          "system_transaction_id": "123456",
          "terminal_id": "190AB",
          "fees": {
            "internal_fee": 10,
            "internal_cashback": 0
          }
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        === "Status code"
            `HTTP 201 Created`

        === "Response body"
            ```json
            {
              "transaction_id": "390IDFE2",
              "system_transaction_id": "123456",
              "created_at": "2022-05-23T12:36:23",
              "cheque_content": [
                {
                  "label": "Created At",
                  "value": "23 May 2022 at 12:36",
                  "position": 1,
                  "section": "main"
                },
                {
                  "label": "Description",
                  "value": "Cash In for @fshevchenko account",
                  "position": 2,
                  "section": "main"
                },
                {
                  "label": "iumiCash Transaction",
                  "value": "390IDFE2",
                  "position": 3,
                  "section": "main"
                },
                {
                  "label": "Terminal transaction",
                  "value": "123456",
                  "position": 4,
                  "section": "main"
                },
                {
                  "label": "Terminal",
                  "value": "190AB",
                  "position": 5,
                  "section": "main"
                },
                {
                  "label": "iumiCash fee",
                  "value": "0.10 $SBD",
                  "position": 6,
                  "section": "total"
                },
                {
                  "label": "Amount",
                  "value": "9.90 $SBD",
                  "position": 7,
                  "section": "total"
                },
                {
                  "label": "Total",
                  "value": "10.00 $SBD",
                  "position": 8,
                  "section": "total"
                }
              ]
            }
            ```
