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

    `id` *UUID* **unique**
    
    :    iumiCash identifier. Using to unique identify resourses (in this example, order etc.)

    `created_at` *datetime*
    
    :    Created time of the order in ISO format.

    `updated_at` *datetime*
    
    :    Updated time of the order in ISO format.

    `username` *string* **unique**
    
    :    User's username.

    `first_name` *string*
    
    :    User's first name.

    `last_name` *string*
    
    :    User's last name.


### Examples

???+ example "Examples"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X GET https://terminal-api.iumi.cash/api/v1/users/my_username/ \
        -H "Authorization: Basic <base64 encoded username:password>"
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
        {
          "id": "42481508-af81-43b9-82dd-d47d9e040ece",
          "external_id": "123456",
          "created_at": "2022-09-12T13:27:09.860466",
          "updated_at": "2022-09-12T13:27:09.860466",
          "status": "created",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ],
          "application_context": {
            "callback_url": "https://starlink.com/api/v1/iumiCash/callback/"
            "success_url": "https://starlink.com/success.html",
            "error_url": "https://starlink.com/error.html"
          },
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "self",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/checkout/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "approve",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/capture/",
              "rel": "capture",
              "method": "post"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/cancel/",
              "rel": "cancel",
              "method": "post"
            }
          ]
        }
        ```

## Callback data

Order information that the iumiCash sends to the vendor's `callback_url`.

Callback data will contain order object, so you can check order status in a iumiCash.

The iumiCash backend will send `POST` request to `callback_url` with `iumicash-signature` header.

!!! Info

    This endpoint should return `200 OK` with content `OK`. Otherwise, the iumiCash will make retry
    requests (once a hour etc.) while the vendor returns successfull response. 
    It required to make sure that the vendor processes order data.

!!! danger "Security tip"

    The iumiCash will send additional `iumicash-signature` header parameter that contains data signature
    signed with `client_secret` (see [client secret] for more information).
    
    So your backend can verify that request was made by the iumiCash.


### Example

???+ example "Examples"

    === "Request"

        Example request with cURL that the iumiCash backend will send to your [`callback_url`](#callback-data).

        ```bash
        curl -v -X POST https://starlink.com/api/v1/iumiCash/callback/ \
        -H "Content-Type: application/json" \
        -H "iumicash-signature: <some-calculated-order-specific-signature>" \
        -d ' \
        {
          "id": "42481508-af81-43b9-82dd-d47d9e040ece",
          "external_id": "123456",
          "created_at": "2022-09-12T13:27:09.860466",
          "updated_at": "2022-09-12T13:27:09.860466",
          "status": "created",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ],
          "application_context": {
            "callback_url": "https://starlink.com/api/v1/iumiCash/callback/"
            "success_url": "https://starlink.com/success.html",
            "error_url": "https://starlink.com/error.html"
          },
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "self",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/checkout/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "approve",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/capture/",
              "rel": "capture",
              "method": "post"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/cancel/",
              "rel": "cancel",
              "method": "post"
            }
          ]
        }
        '
        ```

    === "Response"

        A successful request have to return the `HTTP 200 OK` status code and `OK` content.

        ```python
        def post(request: Request) -> Response:
            iumicash_signature = request.headers.get("iumicash-signature")
            order_data = request.data
            ...
            some business logic and validation
            ...
            return Response(data="OK", status_code=status.HTTP_200_OK)
        ```


## Objects

Request and response objects


### item

???+ info "Description"

    `name` *string*
    :   Name of the order
    
    `description` *string*
    :   Description of the order
    
    `amount` [*object*](#amount)
    :   Amount object
    
    `count` *integer*
    :   Count of this item


### amount

???+ info "Description"

    `value` *Decimal*
    :   amount
    
    `currency_code` *enum*. See [available currencies](#currencies).
    :   Currency of item


### application_context

???+ info "Description"

    `callback_url` *URL string*
    :   Later, the iumiCash will send [callback data](#callback-data) to this endpoint. Callback data will contain order object, so you can check order status in a iumiCash.
    
        This endpoint should return `200 OK` with content `OK`. Otherwise, the iumiCash will make retry
        requests (once a hour etc.) while the vendor returns successfull response. It required to make sure that the vendor processes order data.
    
    `success_url` *URL string*
    :   When order succeeds the iumiCash will redirect user to this url.
    
    `error_url` *URL string*
    :   When order failed somehow (user cancels order etc.) the iumiCash will redirect user to this url.


### HATEOAS

???+ info "Description"

    `link` *URL string*
    :   The complete target URL. To make the related call, combine the method with this `link`.
    
    `rel` *enum*
    :   The link relation type.
        
        Possible values:
       
        * `self` - do `method` with current resourse
        * `approve`
        * `capture`
        * `cancel`

    `method` *enum*
    :   The HTTP method required to make the related call. 
        
        Possible values:
        
        * `GET`
        * `POST`


### Currencies

???+ info "Description"

    Possible values:

    * `SBD`
    * `NZD`

[idempotency]: /idempotency/
[client secret]: /authentication/vendor_registration/
