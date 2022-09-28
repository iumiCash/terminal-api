# Cheque generation

The receipt is sending in the `cheque_content` field in several responses. 
This is the json values code that the terminal renders after inserting `cheque_content` between the constant header and footer.


!!! Tip
    For more information about request see [cash in transaction](/transactions/cash_in/).


### Examples

???+ example "Examples"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://api.iumi.cash/api/v1/orders/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded username:password>" \
        -d ' \
        {
          "username": "fshevchenko",
          "amount": "10.00",
          "internal_fee": "0.10",
          "terminal_id": "190AB"
        }
        '
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
        {
          "transaction_id": "390IDFE2",
          "created_at": "2022-05-23T12:36:23",
          "cheque_content": {
            "created_at": {
              "label": "Created At",
              "value": "23 May 2022 at 12:36"
            },
            "description": {
              "label": "Description",
              "value": "Cash In for @fshevchenko account"
            },
            "transaction_id": {
              "label": "Transaction",
              "value": "390IDFE2"
            },
            "terminal_id": {
              "label": "Terminal",
              "value": "190AB"
            },
            "internal_fee": {
              "label": "Fee 1%",
              "value": "0.10 $SBD"
            },
            "total": {
              "label": "Total",
              "value": "10.00 $SBD"
            }
          }
        }
        ```

### Cheque example

<div class="container">
  <div class="logo-container">
    <img class="logo"
         src="https://iumi.cash/wp-content/uploads/2022/07/cropped-Logo-COL-1-2-2496360276-1658144590803.png"
         alt="logo">
  </div>
  <div class="row-item">
    <p class="row-item_text">Created At:</p>
    <p class="row-item_value">23 May 2022 at 12:36</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Description:</p>
    <p class="row-item_value">Cash In for @fshevchenko account</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Transaction:</p>
    <p class="row-item_value">390IDFE2</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Terminal:</p>
    <p class="row-item_value">190AB</p>
  </div>
  <div class="separator"></div>
  <div class="row-item">
    <p class="row-item_text">Fee 1%:</p>
    <p class="row-item_value">0.10 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Total:</p>
    <p class="row-item_value">10.00 $SBD</p>
  </div>
  <div class="separator"></div>
  <div class="copyright">
    <p class="contacts_text">Contact details: +6104123123 @Andrew Taylor</p>
    <p class="copyright_text">Copyright 2022</p>
  </div>
</div>


<style>
  .container * {
      box-sizing: border-box;
      font-family: 'Roboto', sans-serif;
  }


  .container p {
      margin: 0;
      padding: 0;
  }


  .container {
      width: 300px;
      padding: 30px 10px;
      justify-content: center;
      border: red 1px dashed;
  }

  .logo-container {
      display: flex;
      justify-content: center;
  }

  .logo {
      width: 120px;
      margin-bottom: 20px;
  }

  .row-item {
      display: flex;
      justify-content: space-between;
      flex-wrap: nowrap;
      margin-bottom: 10px;
  }

  .row-item_text {
      font-size: 13px;
  }

  .row-item_value {
      font-size: 13px;
      align-self: flex-end;
      margin-left: 10px;
  }

  .separator {
      margin: 12px 0;
      border-bottom: 1px dashed;
  }

  .copyright {

  }

  .contacts_text {
      font-size: 12px;
  }

  .copyright_text {
      margin-top: 4px;
      font-size: 12px;
      text-align: center;
  }
</style>



