# Fee policies

!!! success "Rule condition"
    You need to use the rule, where user $amount$ is statisfied the next condition:
    
    $more\_than$ $\leqslant$ $amount$ $<$ $less\_than$

The final formula to calculate total sum of a transaction:

!!! note "Abbreviations"

    * TS = total_sum
    * EFF = external_fee_fix
    * IFF = internal_fee_fix
    * ECF = external_cashback_fix
    * ICF = internal_cashback_fix
    * EFP = external_fee_in_percents
    * IFP = internal_fee_in_percents
    * ECP = external_cashback_percents
    * ICP = internal_cashback_percents

### Total sum formula

!!! warning "Direct formula for calculating the total sum"

    $$
    \begin{aligned}
    TS & = amount + (EFF\ ||\ 0)  + (IFF\ ||\ 0) + (ECF\ ||\ 0) + (ICF\ ||\ 0) \\
    & + (amount*(EFP\ ||\ 0)/100)\ + (amount*(IFP\ ||\ 0)/100) \\
    & + (amount*(ECP\ ||\ 0)/100)\ + (amount*(ICP\ ||\ 0)/100)
    \end{aligned}
    $$


!!! danger
    Final amount of money that user will receive to terminal differs from actual input amount.
    That's why the main purpose is calculating the amount which need to be topped up.

### Amount formula

!!! success "The inverse formula for calculating the amount"
    $$
    \begin{aligned}
    amount = \frac{TS - (EFF\ ||\ 0) - (IFF\ ||\ 0) - (ECF\ ||\ 0) - (ICF\ ||\ 0)}
    {1 + (EFP\ ||\ 0)/100 + (IFP\ ||\ 0)/100 + (ECP\ ||\ 0)/100 + (ICP\ ||\ 0)/100}
    \end{aligned}
    $$
