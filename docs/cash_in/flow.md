# Flow diagram

!!! success Tip
    
    For more information see [Mermaid.js](https://squidfunk.github.io/mkdocs-material/reference/diagrams/)
    
```mermaid
graph TD
    start[Start] --> input_username[/"Input @username"/];
    input_username --> check_username{"Is @username exists?"};
    check_username --> |Yes| username_exist["Receive username info"];
    check_username -->|No| username_not_exist["404 page"] --> start;
    
    username_exist --> username_approving("Click Approve button");
    username_approving --> waiting_for_deposit[["Waiting for money deposit"]];
    waiting_for_deposit --> next_after_money_deposit("Click Next button");
    next_after_money_deposit --> deposit_request["Make deposit request"];
    deposit_request --> |"HTTP 4xx"| error_deposit_request["Deposit errors"];
    error_deposit_request -- "After 10 sec redirect to" --> start;
    deposit_request --> |"HTTP 201 Created"| successful_deposit_request["Received transaction details"];

    click start href "#start";
```



## Block descriptions

### start

Here we go again!
