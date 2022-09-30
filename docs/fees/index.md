# Fees - Introduction

This section will describe the main interactions with commissions for different types of services.

To get a list of commissions for all types of services, you need send request:

## Fees

`GET /api/v1/fees/`

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.

### Response

???+ success "Response"

    `fees` *list of [fees](/fees/fees)*
    :    Array of all service fees

!!! note
    See [*Fees*](/fees/fees) page for more details about `fee` value element.

