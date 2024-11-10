# The Vault Screen
* The Raindex app provides a separate screen for navigating the vaults
* This enables users to manage funds within the vaults separately from the associated orders
* We can access a vault by clicking on the `Vaults` tab on the sidebar and then selecting the vault that we want to manage, with the above-mentioned `Network`, `OrderBook`, and `Accounts` filters available to navigate the vault list
<img src="/img/raindex/raindex_vaults_screen.png" /> 
- Alternatively, we can navigate to any vault from the order screen by simply clicking on the vault visible in the top section of the order detail screen

## Top Section - Vault Summary and Balance History

### Browsing Vault Details
* The top left section shows the vault ID of the vault
* Then the orderbook under which the vault was created, vault owner (the wallet that created the vault), and the timestamp when the vault was added
* The token address tells us which token this vault is holding
* The balance tells us the vault holdings denominated in the token of the token address
* `Orders as input` and `Orders as output` show all orders deployed to this vault
* `Orders as input` are orders where the token that the vault is denominated in is being sold, e.g., sell `USDC` for `WETH` 
* `Orders as output` are orders where the token that the vault is denominated in is being bought, e.g., buy `WETH` for `USDC` 
* We can also see the balance history chart on the right which shows how the vault balances changed over time
* For example, an active order vault with real assets would look like this: 
<img src="/img/raindex/raindex_usd_vault.png" /> 

* We can see the token balance change over time as the strategy trades

## Bottom Section - Vault Balance Changes
* In the bottom section, we see `Vault Balance Changes` which lists the transactions that contributed to changes in vault balance
* The `BALANCE CHANGE` column represents the change in balance amount of the vault; if the figure in the balance change column is negative, then the vault balance decreased by that amount; if the figure is positive, then the vault balance increased by that amount
* The `BALANCE` column shows the balance of the vault after that particular transaction was executed
* The `TYPE` column is the type of transaction that contributed to the vault balance change
* If the type of transaction is `Deposit`, then the transaction represents a deposit transaction where funds were added to the vault
* If the type of transaction is `Withdraw`, it represents a withdraw transaction where funds were removed from the vault
* If the type of transaction is `TradeVaultBalanceChange`, then the vault balance was changed as a result of a trade being executed under the orders for which the vault is present as either input or output vault
<img src="/img/raindex/raindex_vaults_bal_change.png" /> 

## Depositing and Withdrawing
* A vault owner, and ***`only`*** a vault owner, can deposit or withdraw funds from a particular vault
* They do this by clicking the `Deposit` or `Withdraw` button available on the vault screen
* These buttons are only accessible to that particular vault owner
<img src="/img/raindex/raindex_deposit_withdraw.png" /> 

### Depositing
* Click on the deposit button to deposit funds into the vault and input the amount of tokens to deposit
* We can also see the current vault balance for the token along with the current token wallet balance present in the connected wallet
* Input the amount to be deposited, and click proceed
* Once you click proceed, you'll be prompted to confirm two transactions on the connected wallet: one is the approve ERC20 transaction, which approves the amount of tokens to be deposited, and the second is the deposit transaction which deposits the tokens in the vaults
<img src="/img/raindex/raindex_vault_deposit.png" /> 
* Once both transactions are confirmed, the vault balance is visible in the vault
<img src="/img/raindex/raindex_vault_deposit2.png" /> 
* For the above vault, we can see the two deposit transactions for `100` and `123` USDC and combined balance of `223` USDC within the vault, and the balance chart is updated as well

### Withdrawing
* To withdraw funds from the vault, simply click on the `Withdraw` button and enter the target amount to be withdrawn and click on the proceed button
* Once the transaction is confirmed, the funds equaling the input amount will be withdrawn from the vault

<img src="/img/raindex/raindex_vault_withdraw.png" /> 
<img src="/img/raindex/raindex_vault_withdraw2.png" /> 
- In the above vault, we can see a withdrawal of `50` USDC made from the vault and the vault balance is updated from `223` to `173` USDC; the same is also reflected by the vault balance chart
