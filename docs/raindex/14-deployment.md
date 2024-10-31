# Deployment
This page provides a detailed guide to deploying and managing orders on the Raindex self-hosted app. Users can :
* Connect supported wallets to the raindex app for submitting transactions.
* Choose specific deployments for order and add orders to the OrderBook
* Apply filters to refine their order search, track and review active orders in real time, analyze order details and historical trading data with all the necessary metrics.
* Monitor and manage funds present in order vaults.

On Raindex owners have control over deposits, withdrawals, and order removal.

# Deploying an order
Let's see how we can deploy `limit-order` with the raindex app. First open the raindex app and navigate to the settings tab on the sidebar and load the following settings:
```
networks:
  base:
    rpc: https://mainnet.base.org
    chain-id: 8453
    network-id: 8453
    currency: ETH

subgraphs:
  base: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/ob4-base/0.9/gn

metaboards:
  base: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/mb-base-0x59401C93/0.1/gn

orderbooks:
  base:
    address: 0xd2938e7c9fe3597f78832ce780feb61945c377d7
    network: base
    subgraph: base

deployers:
  base:
    address: 0xC1A14cE2fd58A3A2f99deCb8eDd866204eE07f8D
    network: base
```
These are the same settings we used before, the settings should be applied successfully.
<!-- ![raindex_settings](https://hackmd.io/_uploads/rkM-GoyWyl.png) -->
<img src="/img/raindex/raindex_settings.png" />  

Next navigate to raindex editor by clicking on the `New Order` tab on the sidebar and paste the following in the raindex editor: 
```
# Buy Weth : NOT TO BE USED FOR TRADING REAL ASSETS

raindex-version : db14c87f012a76980661802ff424371d6e84552e

tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
  base-usdc:
    network: base
    address: 0xA72e7279157b9840e5B4b911d142416F80fcDf07

orders:
  base-weth-buy:
    orderbook: base
    inputs:
      - token: base-weth
        vault-id: 0x4767d92a5f01500424d2a2dd88964314f8a98a6b66bcf1db362b0ad9006c93e8
    outputs:
      - token: base-usdc
        vault-id: 0x4767d92a5f01500424d2a2dd88964314f8a98a6b66bcf1db362b0ad9006c93e8

scenarios:
    limit-order:
      network: base
      deployer: base
      orderbook: base
      bindings:
        orderbook-subparser: 0x662dFd6d5B6DF94E07A60954901D3001c24F856a
        
      scenarios:
        buy:
          bindings:
            # io-ratio and amount for the limit order.
            limit-ratio: 0.0004
            output-amount: 100
            
          scenarios:
            prod:
              bindings:
                get-output-amount: '''get-output-amount-prod'
            plot:
              runs: 100
              bindings:
                get-output-amount: '''get-output-amount-plot'
            backtest:
              runs: 1
              blocks:
                range: [21530860..21531860]
                interval: 100
              bindings:
                get-output-amount: '''get-output-amount-prod'

charts:
  A. Limit order metrics :
    scenario: limit-order.buy.prod
    metrics:
      - label: Maxmimum USDC offered
        value: 0.2
        unit-suffix: " USDC"
        description: 'Maximum USDC offered by the order.'
      - label: Order ratio
        value: 0.3
        unit-suffix: " WETH/USDC"            
        description: 'Order ratio at which WETH was purchased.'

  B. Limit order simulation:
      scenario: limit-order.buy.plot
      plots:
        Buy WETH simulation:     
          x:
              label: 'USDC Output'
          y:
              label: 'WETH/USDC Ratio'
          marks:
            - type: line
              options:
                x: 0.2
                y: 0.3 
  C. Limit order backtest:
      scenario: limit-order.buy.backtest
      plots:
        Buy WETH backtest:     
          x:
              label: 'Block Number'
          y:
              label: 'WETH/USDC Ratio'
          marks:
            - type: line
              options:
                x: 0.1
                y: 0.0  


deployments:
  base-weth-buy:
    scenario: limit-order.buy.prod
    order: base-weth-buy
---
#orderbook-subparser !The subparser for the Orderbook words

#limit-ratio !IO ratio for the limit order.
#output-amount !Output amount for the limit order.

#test-output-amount !Binding to test maximum output amount.
#get-output-amount !Binding to get order output max.

#get-output-amount-prod
 max-output: output-amount;

#get-output-amount-plot
 max-output: mod(test-output-amount 100);

#calculate-io
  using-words-from orderbook-subparser 0xD6B34F97d4A8Cb38D0544dB241CB3f335866f490
  
  current-price: uniswap-v3-quote-exact-input(
    0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913 0x4200000000000000000000000000000000000006 
    1
    0x33128a8fC17869897dcE68Ed026d694621f6FDfD [uniswap-v3-init-code]
    [uniswap-v3-fee-medium]
  ),
  _: block-number(),
  final-amount: call<'get-output-amount>(),
  final-ratio: limit-ratio;
  
#handle-io
  :;

#handle-add-order
 :;
```
Alternatively, you can also click on the `Load` button present on the top right corner of the editor and load the `limit-order.rain` file from the local machine if you have it saved before.

<img src="/img/raindex/raindex_load_file.png" />  

## Connect wallet
Next lets connect the wallet to the raindex app. You can  connect your wallet by clicking `Connect to Wallet` button in the sidebar, a modal will pop up giving you the options to connect a Ledger wallet or connect via WalletConnect.
<img src="/img/raindex/raindex_connect_wallet.png" />  

- If you have a ledger wallet, you can select the `Ledger Wallet` button from the modal. Enter the correct derivation index for the ledger wallet and click on the `Connect` button.
<img src="/img/raindex/raindex_ledger_wallet.png" />  

- Alternatively you can connect your wallet via `WalletConnect`. Click on the `WalletConnect` option from the modal, click on the `Connect` button and scan the wallet connect QR code from your wallet to connect.
<img src="/img/raindex/raindex_wallet_connect.png" />  


- Once your wallet is connect you'll see the connected wallet's address preview on the sidebar panel.
<img src="/img/raindex/raindex_connect_preview.png" />  
   
- Finally open the `Settings` tab again and add the `accounts` top level element and paste you wallet address under an identifier key. For example : 
```yaml
accounts:
  mobile-wallet: 0xCA2Dac87aD301250D7D69f50Cf0890Bc5d6Ca994
```
<img src="/img/raindex/raindex_settings_account.png" />  

## Deployments
Each dotrain file has `deployments` top level element in the yaml that defines `order` (consisting of input and output vaults) and associated `scenario`. The rainlang composition for this scenario is then converted to bytecode and is passed as part of the order.
For our `limit-order` example the bytecode for composed `limit-order.buy.prod` scenario will be part of the `base-weth-buy` along with the input and output vaults of the order. 
```
deployments:
  base-weth-buy:
    scenario: limit-order.buy.prod
    order: base-weth-buy
```
After connecting your wallet, scroll to the bottom of the editor and select the deployment to be submitted and click on the `Add Order` button. 
<img src="/img/raindex/raindex_add_order.png" />  


A modal will pop up, click on the `Add Order` button on the prompt and confirm the transaction in your wallet.
<img src="/img/raindex/raindex_confirm_add_order.png" />  

 
Wait for the order to be confirmed, the order is now deployed!


# Browsing orders
Now that the order is added to the orderbook, the transaction is confirmed on chain and indexed by the indexer. We can browse all the orders by navigating to the `Orders` tab on the sidebar where we can see all the added orders added to the orderbook.
<img src="/img/raindex/raindex_all_orders.png" />  
Sometimes it may happen that the orderbook has many orders added under the orderbook and it becomes diffcult to find our order, but we can search for our order by using filters.

# Searching orders
* Once we navigate to the `Orders` tab we can see a list of filter available at the top of the screen which we can use to filter out the order that we want to see. 
* You can search for existing orders by entering the order-hash in the search box, an order-hash is a unique id generated for the order at the time of deployment. Order hash for the order can be know by quering the indexer associated with the orderbook.

# Filtering orders
* You can filter by order status, account (owner list), network and order book filter present at the top of the screen.
* Filter by active or inactive orders using the status drop down
<img src="/img/raindex/raindex_order_status_filter.png" />  

* You can also use the accounts filter to filter out orders added by that particular account
<img src="/img/raindex/raindex_account_filter.png" />  

* The `Orderbook` and `network` filters filter out the orders added under the paritcular OrderBook contract for that specific network.
<img src="/img/raindex/raindex_network_ob_filter.png" />  

 

# Understanding the order screen
By clicking on a order hash throughout raindex, you will get taken to the order detail screen.

## Top section - Order details

### Browsing order details
<img src="/img/raindex/raindex_order_screen_intro.png" />  

* The top left section shows the order-hash of the order along with its current status `Active`  or `Inactive` 
* Then the orderbook under which the order was deployed, order owner (the wallet that deployed the order) and the timestamp when the order was added
* We can also see the input and the output vaults associated with the order, in this particular case these are `WETH` and `USDC` vaults
* Input vaults hold tokens that are being sold in trading strategies e.g. sell `USDC` for `WETH`
* Output vaults hold tokens that are being bought in trading strategies e.g. buy `WETH` with `USDC`
* An order can have multiple inputs and output vaults associated with it.
* Top right we can see a trading graph which gives a visual depiction of price once the order starts trading, Eg: Following is the vault screen of an actively trading order : 

<img src="/img/raindex/raindex_order_top.png" />


## Middle section - order quotes
* Within `Order Quotes` we can see the order pair and the maximum output amount offered by the order along with its price.
* The maximum input amount is the input token amount received for offering the output tokens.
* If the order adjusts it's price by pinging an on-chain oracle, the quote prices may change from block to block. In order to view what price is offered at a particular block, we can use the debugger tool by click on the debug icon which will give the elements currently present on the stack for that specific block. 
* The stack element may change from block to to block, and the quotes are refreshed as new blocks are mined. By clicking on the refresh and pause icon at the top right of the quotes section we can fetche new block or pause at the current one and run the debugger.
<img src="/img/raindex/raindex_order_debug_quote.png" /> 
- If we recall from our example from reading the stack elements, the debugger show the `calculate-io` stack indexed `0` elements for which are visible here. 
   - Eg `0.3` is the `final-ratio` and `0.2` is the `final-amount` from our rainlang source.

## Bottom section - source, trades and volume
* In the bottom section of the order screen, there are tabs for `Rainlang` source and `Trades` and `Volume` for the order.
* The `Rainlang` tab shows the fully composed rainlang source associated with the order. This is same as the resultant composition of the production scenario mentioned in the deployment section.
* If the rainlang source isn't what we expected or if there is an incorrect binding associated with the source we can view it in this tab before adding funds, or use it to troubleshoot if the strategy doesn't perform as expected.
<img src="/img/raindex/raindex_order_screen_rainlang.png" /> 
* The `Trades` tab gives an overview of the order trades with details like trade timestamp, trade counterparty, transaction hash, amount of input tokens received, amount of output tokens offered and the i/o ratio.
* We can view historical trades broken into complete history and last 48 and 24 hour by applying the trade filter.

<img src="/img/raindex/raindex_order_trades_tab.png" /> 

* The volumes tabs show the token trade volumes for the vaults associated with the order
* Metrics like net volume and total volume are visible for the same 24-48 hours/all time period.
* We can view historical volumes broken into complete history and last 48 and 24 hour by applying the volume filter.
* `In Volume` is the total amount of input tokens received by aggregating all the trades under this order.
* `Out Volume` is the total amount output tokens offered for all the trades under the order.
* `Net Volume` is the difference between in and out volume for the token
* `Total Volume` is the sum of in and out volume for the token
<img src="/img/raindex/raindex_order_vol_tab.png" /> 

## Removing an order
* Order owners, and ***`only order owners`*** can remove any active order by simply clicking the `Remove` button on the top right of the top section and confirming the transaction.

<img src="/img/raindex/raindex_remove_order_button.png" /> 

Once the order is removed
* The order detail screen, order lists, vault lists and vault detail screens will show `Inactive` instead of `Active` 
* The remove button will no longer be visible
* The order will stop executing

<img src="/img/raindex/raindex_order_inactive.png" /> 

# Understanding the vault screen
* The raindex app provides a separate screen for navigating the vaults
* This enables users to manage funds within the vaults separately from the associated orders.
* We can access a vault by clicking on the `Vaults` tab on the sidebar and then selecting the vault that we want to manage, with the above mentioned `Network`, `OrderBook` and `Accounts` filter available to navigate the vault list.
<img src="/img/raindex/raindex_vaults_screen.png" /> 
- Alternatively we can navigate to any vault from the order screen by simply clicking on the vault visible in the top section of the order detail screen. 

## Top Section - vault summary and balance history

### Browsing vault details
* The top left section shows the vault ID of the vault
* Then the orderbook under which the vault was created, vault owner (the wallet that created the vault) and the timestamp when the vault was added
* The token address tells us which token this vault is holding
* The balance tells us the vault holdings denominated in the token of the token address
* `Orders as input` and `Orders as output` show all orders deployed to this vault
* `Orders as input` are orders where the token that the vault is denominated in, is being sold e.g. sell `USDC` for `WETH` 
* `Orders as output` are orders where the token that the vault is denominated in, is being bought e.g. buy `WETH` for `USDC` 
* We can also see the balance history chart on the right which shows how the vault balances changed over time
* Example, an active order vault with read assets would look like this: 
<img src="/img/raindex/raindex_usd_vault.png" /> 

* We can see the token balance change over time as the strategy trades

## Bottom Section - vault balance changes
* In the bottom section, we see `Vault Balance Changes` which lists the transaction that contributed to change in vault balance.
* The `BALANCE CHANGE` column represents the change in balance amount of the vault, if the figure in the balance change column is negative the the vault balance decreased by that amount, if the figure is positive then the vault balance increased by that amount.
* The `BALANCE` column shows the balance of the vault after that particular transaction was executed.
* The `TYPE` column is type of transaction that contributed to the vault balance change.
* If the type of transaction is `Deposit` then the transaction represent deposit transaction where funds were added to the vault.
* If the type of transaction is `Withdraw` it represents withdraw transaction where funds were removed from the vault.
* If the type of transaction is `TradeVaultBalanceChange` then the vault balance was changed as a result of trade being executed under the orders for which the vault is present as either input or output vault.
<img src="/img/raindex/raindex_vaults_bal_change.png" /> 
## Depositing and withdrawing
* A vault owner, and ***`only`*** a vault owner, can deposit or withdraw funds from the a particular vault.
* They do this by clicking the `Deposit` or `Withdraw` button avaialable on the vault screen
* These buttons are only accessible to that particular vault owner.
<img src="/img/raindex/raindex_deposit_withdraw.png" /> 

### Depositing
* Click on the deposit button to deposit funds into the vault and input the amount of tokens to deposit. 
* We can also see the the current vault balance for the token along with the current token wallet balance present in the connected wallet.
* Input the amount to be deposited, and click proceed.
* Once you click proceed, you'll be prompted to confirm two transaction on the connected wallet one is the approve ERC20 transaction, which apporves the amount of tokens to be deposited, and the second is the deposit transaction which deposits the tokens in the vaults.
<img src="/img/raindex/raindex_vault_deposit.png" /> 
* Once both the transactions are confirmed the vault balance is visible in the vault.
<img src="/img/raindex/raindex_vault_deposit2.png" /> 
* For the above vault, we can see the two deposit transaction for `100` and `123` USDC and combined balance of `223` USDC within the vault and the balance chart is updated as well.

### Withdrawing
* To withdraw funds from the vault, simply click on the `Withdraw` button and enter the target amount to be withdrawn and click on the proceed button.
* Once the transaction is confirmed the funds equalling the inputed amount will be withdrawn from the vault.

<img src="/img/raindex/raindex_vault_withdraw.png" /> 
<img src="/img/raindex/raindex_vault_withdraw2.png" /> 
- In the above vault, we can see a withdrawal of `50` USDC made to the vault and the vault balance is updated from `223` to `173` USDC, the same is also reflected by the vault balance chart.





