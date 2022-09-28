# Fees

This section will describe the fee calculation policy for different types of services. 
The fee structure is always the same, only the values differ. 
The detailed information of this structure is described below.

### Response

???+ success "Response"

    `key` *string* **unique**
    
    :    Transaction type key

    `rules` **list of [*rule*](#rule)**
    
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

???+ success "Rule object"

    `less_than` *integer*
    
    :   The amount need to be less than or equal the value of this field to accept this rule object.
    :   !!! note
            This value is stored as cents.

    `more_than` *integer*
    
    :   The amount need to be more than the value of this field to accept this rule object.
    :   !!! note
             This value is stored as cents.

    `external_fee_in_percents` *decimal*   

    :    Commission charged by a third party. 
         The commission value is calculated as a percentage of the transaction amount.

     `internal_fee_in_percents` *decimal*   

    :    Commission charged by iumiCash. 
         The commission value is calculated as a percentage of the transaction amount.   
   
     `external_fee_fix` *integer*   

    :    Commission charged by a third party.
         The commission is charged as a fixed value regardless of the transaction amount.
    :   !!! note
             This value is stored as cents.

     `internal_fee_fix` *integer*   

    :    Commission charged by iumiCash.
         The commission is charged as a fixed value regardless of the transaction amount.
    :   !!! note
             This value is stored as cents.

     `external_cashback_in_percents` *decimal*   

    :   Cashback returned by a third party. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note
             This value is negative decimal by default, if exists and not equal 0. 

     `internal_cashback_in_percents` *decimal*   

    :   Cashback returned by iumiCash. The cashback value is calculated as a percentage of the transaction amount.
    :   !!! note
             This value is negative decimal by default, if exists and not equal 0. 

     `external_cashback_fix` *integer*   

    :   Cashback returned by a third party. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note
             * This value is negative decimal by default, if exists and not equal 0. 
             * This value is stored as cents.

     `internal_cashback_fix` *integer*   

    :   Cashback returned by iumiCash. The cashback is charged as a fixed value regardless of the transaction amount.
    :   !!! note
             * This value is negative decimal by default, if exists and not equal 0. 
             * This value is stored as cents.




Imagine that a user wants to pay $amount$ dollars. If $more\_than$ $\leqslant$ $amount$ $<$ $less\_than$, the rule is acceptable.

The final formula to calculate total sum of a transaction:

!!! note "Abbreviation"

    * TS = total_sum
    * EFF = external_fee_fix
    * IFF = internal_fee_fix
    * ECF = external_cashback_fix
    * ICF = internal_cashback_fix
    * EFP = external_fee_in_percents
    * IFP = internal_fee_in_percents
    * ECP = external_cashback_percents
    * ICP = internal_cashback_percents

!!! warning "Not right"

    $$
    \begin{aligned}
    TS & = amount + (EFF\ ||\ 0)  + (IFF\ ||\ 0) + (ECF\ ||\ 0) + (ICF\ ||\ 0) \\
    & + (amount*(EFP\ ||\ 0)/100)\ + (amount*(IFP\ ||\ 0)/100) \\
    & + (amount*(ECP\ ||\ 0)/100)\ + (amount*(ICP\ ||\ 0)/100)
    \end{aligned}
    $$


!!! danger
    Final amount of money that user will receive to its account differs from actual input amount.


$$
\begin{aligned}
amount = \frac{TS - (EFF\ ||\ 0) - (IFF\ ||\ 0) - (ECF\ ||\ 0) - (ICF\ ||\ 0)}
{1 + (EFP\ ||\ 0)/100 + (IFP\ ||\ 0)/100 + (ECP\ ||\ 0)/100 + (ICP\ ||\ 0)/100}
\end{aligned}
$$