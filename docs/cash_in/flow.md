# Flow diagram

!!! success Tip
    
    For more information see [Mermaid.js](https://squidfunk.github.io/mkdocs-material/reference/diagrams/)
    
```mermaid
graph TD
    start[Start] --> input_username[/"Input @username"/];
    input_username --> check_username{"Is @username exists?"};
    check_username --> |Yes| username_exist["Receive username info"];
    check_username --> |No| error_page(("4xx error page"));
    error_page -- "After 10 sec redirect to" --> start;
    
    username_exist --> username_approving("Click Approve button");
    username_approving --> waiting_for_deposit[["Waiting for money deposit"]];
    waiting_for_deposit --> next_after_money_deposit("Click Next button");
    next_after_money_deposit --> deposit_request["Make deposit request"];
    deposit_request ----> |"HTTP 4xx"| error_page;
    deposit_request --> |"HTTP 201 Created"| successful_deposit_request["Received transaction details"];

    click start href "#start";
    click input_username href "#input_username";
    click check_username href "#retrieve_user";
    click username_exist href "#username_exist";
    click error_page href "#error_page";
    click username_approving href "#username_approving";
    click waiting_for_deposit href "#waiting_for_deposit";
    click next_after_money_deposit href "#next_after_money_deposit";
    click deposit_request href "#deposit_request";
    click successful_deposit_request href "#successful_deposit_request";
```



## Block descriptions

### start

The initial screen with the ability to enter username and click on the "Next" button

### input_username

The user enters username and clicks on the "Next" button

### retrieve_user

The terminal system sends a request to the iumiCash server at `GET /api/v1/users/<username:str>/`.

See [This link](/users/retrieve/) for more information about this request.

### username_exist

If Response from `GET /api/v1/users/<username:str>/` returned `200 OK`, terminal displays username details,
such as `first_name` and `last_name`. The terminal waits for one minute until the user clicks the "Next" button.

### error_page

# ToDo

If Response from `GET /api/v1/users/<username:str>/` returned `4xx Status Code`, terminal displays error details.
(`User Not Found`, `User is deactivated`, etc.) and returns user to the [start page](#start) after 10 seconds.

### username_approving

The client makes sure that the `first_name` and `last_name` matches the person whose username he entered and click the "Approve" button.

### waiting_for_deposit

The terminal waits for the user to enter money.

### next_after_money_deposit

After the client has entered a sufficient amount for Cash In, he clicks on the "Next" button.

### deposit_request

The client is shown the final information about the inserted data, total sum and commission.
Loading is also output to get the status and details of the transaction.
In parallel, the terminal system sends a CashIn request to the iumiCash backend.

### successful_deposit_request

# Todo
