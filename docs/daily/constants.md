# Constants

## Constants API

Get constants request.

`GET /api/v1/constants/`


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the basic authorization in this header with the `Basic` authentication scheme. 
         The value is `Basic <base64string username:password>`


### Response

???+ success "Response"

    `logo` *URL string*
    
    :    iumiCash logo .png url

    `available_payments` *list of [*payments*][payments]*

    :    List of available payments keys

    `cheque_footer` *string*
    
    :    Text for cheque footer which is printed by terminal

    `fees` *list of [*fee*][fees]*
    
    :    The actual fees for each transaction type


[payments]: ../payments/payments.md
[fees]: ../fees/index.md
