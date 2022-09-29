# Flow diagram


```mermaid
flowchart TD
    start([Start]) ----> input_username[\"Input username"/];
    
    subgraph username_validation ["Username validation"];
        input_username --> check_username_request>"Check username request"];
        check_username_request --> |HTTP 4xx-5xx| error_page(["HTTP 4xx: Not Exist"]);
        error_page --> input_username;
        check_username_request ----> |HTTP 200 OK| show_username_info["Show username info"];
    end;
    
    show_username_info -- "Click Approve button" ----> insert_money[\"Insert money"/];
    
    subgraph money_inserting ["Inserting money"];
        insert_money --> show_inserted_money_amount["Show inserted money amount"];
        show_inserted_money_amount --> insert_money;
    end;
    
    show_inserted_money_amount -- "Click Next button" ---> deposit_request>"Make deposit request"];
    deposit_request --> |"HTTP 201 Created"| successful_deposit_request["Received transaction details"];
    deposit_request --> |"HTTP 4xx-5xx"| error_deposit_request["Transaction in progress"];
    successful_deposit_request --> print_cheque[["Print cheque"]];
    error_deposit_request --> print_cheque[["Print cheque"]];

    click start href "#start";
    click input_username href "#input_username";
    click check_username_request href "#check_username_request";
    click show_username_info href "#show_username_info";
    click error_page href "#error_page";
    click insert_money href "#insert_money";
    click show_inserted_money_amount href "#show_inserted_money_amount";
    click deposit_request href "#deposit_request";
    click successful_deposit_request href "#successful_deposit_request";
    click error_deposit_request href "#error_deposit_request";
    click print_cheque href "#print_cheque";
```



## Block descriptions

### start

The initial screen with the ability to enter username and click on the "Next" button

### input_username

The user enters username and clicks on the "Next" button

### check_username_request

The terminal system sends a request to the iumiCash server at `GET /api/v1/users/<username:str>/`.

See [This link](/users/retrieve/) for more information about this request.

### show_username_info

If Response from `GET /api/v1/users/<username:str>/` returned `200 OK`, terminal displays username details,
such as `first_name` and `last_name`. The terminal waits for one minute until the user clicks the "Next" button.

### error_page

This page displays any possible error. For example, if a response does not have `2xx Status code`.

If Response from `GET /api/v1/users/<username:str>/` returned `4xx Status Code`, terminal displays error details.
(`User Not Found`, `User is deactivated`, etc.) and returns user to the [start page](#start) after 10 seconds.

### insert_money

The terminal waits for the user to enter money.

### show_inserted_money_amount

The terminal shows inserted money amount.

### deposit_request

The client is shown the final information about the inserted data, total sum and commission.
Loading is also output to get the status and details of the transaction.
In parallel, the terminal system sends a CashIn request to the iumiCash backend.

See [cash in transaction](/transactions/cash_in/) for request details.

### successful_deposit_request

If transaction received, terminal displays transaction details.

### error_deposit_request

If transaction failed, terminal displays error details.

### print_cheque

Prints cheque using `cheque_content` from [successful deposit request](#successful_deposit_request).

For more details, see [Cheque generation](/cash_in/cheque_generation)
