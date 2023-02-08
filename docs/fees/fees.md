# Fees

This section will describe the fee calculation policy for different types of services. 
The fee structure is always the same, only the values differ. 

The commission calculation is necessary in order to transfer the amount, 
taking into account the deduction of all commissions, cashback, etc. from the amount entered by the user into the terminal.


### Response

???+ info "Fee object"

    `key` *string* **unique**
    :    Transaction type key

    `rules` *list of [*rule*](#rule)*
    :    This field describes which rule element should be applied to the entered amount. Fields of rules element:
    
         * `less_than`
         * `more_than`
         * `external_fee_in_percents`
         * `internal_fee_in_percents`
         * `external_fee_fix`
         * `internal_fee_fix`
         * `external_cashback_in_percents`
         * `internal_cashback_in_percents`
         * `external_cashback_fix`
         * `internal_cashback_fix`

    :   !!! info
            To understand which rule to apply, there are fields `more_than` and `less_than`.
            See [*rule*](#rule) for more details.


### Rule

??? success "Rule object"

    `less_than` [*integer*][cent integer]
    :   The amount need to be less than or equal the value of this field to accept this rule object.

    `more_than` [*integer*][cent integer]
    :   The amount need to be more than the value of this field to accept this rule object.

    `external_fee_in_percents` *decimal*   
    :    Commission charged by a third party. 
         The commission value is calculated as a percentage of the transaction amount.

    `internal_fee_in_percents` *decimal*   
    :    Commission charged by iumiCash. 
         The commission value is calculated as a percentage of the transaction amount.   
   
    `external_fee_fix` [*integer*][cent integer]
    :    Commission charged by a third party.
         The commission is charged as a fixed value regardless of the transaction amount.

    `internal_fee_fix` [*integer*][cent integer]
    :    Commission charged by iumiCash.
         The commission is charged as a fixed value regardless of the transaction amount.

    `external_cashback_in_percents` *decimal*   
    :   Cashback returned by a third party. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note ""
             This value is negative decimal by default, if exists and not equal 0. 

    `internal_cashback_in_percents` *decimal*   
    :   Cashback returned by iumiCash. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note ""
             This value is negative decimal by default, if exists and not equal 0. 

    `external_cashback_fix` [*integer*][cent integer]
    :   Cashback returned by a third party. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note ""
             * This value is negative decimal by default, if exists and not equal 0. 

    `internal_cashback_fix` [*integer*][cent integer]
    :   Cashback returned by iumiCash. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note ""
             * This value is negative decimal by default, if exists and not equal 0. 

## Policies

[Policies](../fees/policies.md) page is describing how to communicate with these rule parameters.


[cent integer]: ../types.md#cent-integer
