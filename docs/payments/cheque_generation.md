## Cheque Refund

If for some reason the iumiCash server has not processed the user's transaction, 
he can always come up with a receipt and receive a refund on this receipt. 
A receipt contains the transaction ID, transaction details and amount of the transaction. 
The agent (or other actor) validates the provided data and marks this transaction as `refunded` 
inside the iumiCash system. 
This is necessary so that the user cannot go with this receipt and get a refund one more time.