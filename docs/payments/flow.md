# Flow diagram

```mermaid
flowchart TD
    start([Start]) --> show_categories["Show menu of categories"];
    show_categories --> choose_category[\"Choose category"/];
    choose_category -- "Click Next button" --> show_grouped_payments["Show menu of grouped payments"];
    show_grouped_payments --> choose_payment[\"Choose payment"/];
    choose_payment -- "Click Next button" --> payment_schema_request>"Request chosen payment schema"];
    payment_schema_request -- "HTTP 200 OK" --> show_payment_screen["Show payment screen"];
    payment_schema_request -- "HTTP 4xx-5xx" --> error_page(["HTTP 4xx error page"]);
    error_page -- "After 10 sec redirect to" --> start;
    show_payment_screen -- "Click Next button" ----> input_user_credentials[\"Input user credentials"/];
    
    subgraph credential_validations ["Credential validation"];
        input_user_credentials --> check_credentials_request>"Check credentials request"];
        check_credentials_request -- "HTTP 4xx-5xx" --> error_invalid_credentials(["HTTP 4xx: Invalid credentials"]);
        error_invalid_credentials --> input_user_credentials;
    end;
    
    check_credentials_request -- "HTTP 200 OK" -----> insert_money[\"Insert money"/];
    
    subgraph money_inserting ["Inserting money"];
        insert_money --> calculate_amount_and_fees["Show calculated amount (money, fees)"];
        calculate_amount_and_fees --> insert_money;
    end;
    
    calculate_amount_and_fees -- "Click Next button" ---> send_payment_request>"Send payment request"];
    send_payment_request --> |HTTP 201 Created| successful_send_payment_request["Transaction details"];
    send_payment_request --> |HTTP 4xx-5xx| error_send_payment_request["Payment in progress"];
    successful_send_payment_request --> print_cheque[["Print cheque"]];
    error_send_payment_request --> print_cheque[["Print cheque"]];

    click start href "#start";
    click show_categories href "#show_categories";
    click choose_category href "#choose_category";
    click show_grouped_payments href "#show_grouped_payments";
    click choose_payment href "#choose_payment";
    click payment_schema_request href "#payment_schema_request";
    click show_payment_screen href "#show_payment_screen";
    click input_user_credentials href "#input_user_credentials";
    click check_credentials_request href "#check_credentials_request";
    click error_invalid_credentials href "#error_page";
    click insert_money href "#insert_money";
    click calculate_amount_and_fees href "#calculate_amount_and_fees";
    click send_payment_request href "#send_payment_request";
    click successful_send_payment_request href "#successful_send_payment_request";
    click error_send_payment_request href "#error_send_payment_request";
    click print_cheque href "#print_cheque";
    click error_page href "#error_page";
```



## Block descriptions

### start

The initial screen with all payment categories.

### show_categories

Each category is clickable and contains specific payments.

### choose_category

The user clicked on the category.

### show_grouped_payments

This screen displays only payments related to the selected category.

### choose_payment

The user clicked on the payment.

### payment_schema_request

The terminal system sends a request to the iumiCash server at `GET /v1/terminal-api/payments/schemas/<key>/`.

See [This link](get.md) for more information about this request.

### error_page

This page displays any possible error. For example, if a response does not have `2xx Status code`.

### show_payment_screen

Terminal renders payment screen based on the provided payment schema.

### input_user_credentials

User inputs payment credentials, such as `phone` if payment is Bmobile/Our Telekom, `login` if payment is Satsol etc.

### check_credentials_request

The terminal system sends a request to the iumiCash server at `POST /v1/terminal-api/payments/check-credentials`.

See [This link](../payments/check_credentials.md) for more information about this request.

### insert_money

Terminal is waiting for inserting user money. Trigger to stop waiting is press "Next" button.

### calculate_amount_and_fees

Since the terminal system knows the policy of calculating all payment fees ([Fees policies](../fees/policies.md)), 
the terminal can display a list of fees every time a banknote is entered.

### send_payment_request

The terminal system sends a request to the iumiCash server at `POST /v1/terminal-api/transactions/payments/`.

See [This link](../transactions/send_payment.md) for more information about this request.

### successful_send_payment_request

If iumiCash responded with `201 Created` request and the transaction status is `Success` (See [*All Transaction Statuses*](../transactions/statuses.md)), 
then the terminal shows the details of the successfully 
completed transaction and proceeds to print the receipt

### error_send_payment_request

If for some reason the iumiCash server did not respond or returned the status not `Success`, 
the terminal must show the details of the transaction and print a receipt, 
according to which the user will be able to refund the money for the unsuccessful transaction 
(More about this: [Cheque refund](../payments/cheque_generation.md#cheque-refund)). At this time, the terminal system should send a request to the 
iumiCash server until it is convinced of the finiteness of the transaction.

!!! note
    Please, pay attention to [idempotency](../idempotency.md), while terminal system is sending the request.

### print_cheque

Prints cheque using `cheque_content` from [successful_send_payment_request](#successful_send_payment_request).

For more details, see [Cheque generation](../payments/cheque_generation.md)
