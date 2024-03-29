# Cheque generation

The receipt is sending in the `cheque_content` field in several responses.
This is the json values code that the terminal renders after inserting `cheque_content` between the constant header and footer.


!!! Tip
    For more information about request see [payment transaction].


### Examples

??? success "Successful request"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://iumi.cash/v1/terminal-api/transactions/payments \
        -H "Content-Type: application/json" \
        -H "Authorization: Basic <base64 encoded email:password>" \
        -H "RequestId: 7b92603e-77ed-4896-8e78-5dea2050476a" \
        -d ' \
        {
          "key": "bmobile",
          "credentials": {
            "phone": "8509813"
          },
          "amount": 1000,
          "system_transaction_id": "123456",
          "terminal_id": "190AB"
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
                  "value": "8509813 recharged with SBD $10 (Transaction ID: 0927165328159047)",
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
                  "value": "0.40 $SBD",
                  "position": 6,
                  "section": "total"
                },
                {
                  "label": "Bmobile fee",
                  "value": "0.30 $SBD",
                  "position": 7,
                  "section": "total"
                },
                {
                  "label": "iumiCash cashback",
                  "value": "0.20 $SBD",
                  "position": 8,
                  "section": "total"
                },
                {
                  "label": "Bmobile cashback",
                  "value": "0.10 $SBD",
                  "position": 9,
                  "section": "total"
                },
                {
                  "label": "Amount",
                  "value": "9.60 $SBD",
                  "position": 10,
                  "section": "total"
                },
                {
                  "label": "Total",
                  "value": "10.00 $SBD",
                  "position": 11,
                  "section": "total"
                }
              ]
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
    <p class="row-item_text">iumiCash Transaction:</p>
    <p class="row-item_value">390IDFE2</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Terminal Transaction:</p>
    <p class="row-item_value">123456</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Terminal:</p>
    <p class="row-item_value">190AB</p>
  </div>
  <div class="separator"></div>
  <div class="row-item">
    <p class="row-item_text">iumiCash fee:</p>
    <p class="row-item_value">0.40 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Bmobile fee:</p>
    <p class="row-item_value">0.30 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">iumiCash cashback:</p>
    <p class="row-item_value">0.20 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Bmobile cashback:</p>
    <p class="row-item_value">0.10 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Amount</p>
    <p class="row-item_value">9.60 $SBD</p>
  </div>
  <div class="row-item">
    <p class="row-item_text">Total:</p>
    <p class="row-item_value">10.00 $SBD</p>
  </div>
  <div class="separator"></div>
  <div class="copyright">
  <p class="copyright_text">Please save cheque until paying for service</p>
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


## Cheque Refund

If for some reason the iumiCash server has not processed the user's transaction,
he can always come up with a receipt and receive a refund on this receipt.
A receipt contains the transaction ID, transaction details and amount of the transaction.

## Failed Cheque Generating

If iumiCash did not respond to the request and as a result did not return the content receipt, 
the terminal must print the receipt so that the user can always get the money back in one form 
or another or contact the support service.

[payment transaction]: ../transactions/send_payment.md
