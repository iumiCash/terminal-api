# Constants

## Constants API

Get constants request.

`GET /v1/terminal-api/constants/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string email:password>`


### Response

???+ success "Response"

    `logo` *URL string*
    
    :    iumiCash logo .png url

    `cheque_footer` *string*
    
    :    Text for cheque footer which is printed by terminal


[payments]: ../payments/payments.md
[fees]: ../fees/index.md
