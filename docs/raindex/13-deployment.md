# Deployment
This page provides a detailed guide to deploying and managing orders on the Raindex self-hosted app. Users can:
* Connect supported wallets to the Raindex app for submitting transactions.
* Choose specific deployments for orders and add orders to the OrderBook
* Apply filters to refine their order search, track and review active orders in real-time, analyze order details, and historical trading data with all the necessary metrics.
* Monitor and manage funds present in order vaults.

On Raindex, owners have control over deposits, withdrawals, and order removal.

# Deploying an order
Let's see how we can deploy a `limit-order` with the Raindex app. First, open the Raindex app and navigate to the settings tab on the sidebar and load the following settings:
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
These are the same settings we used before; the settings should be applied successfully.

<img src="/img/raindex/raindex_settings.png" />  

Next, navigate to the Raindex editor by clicking on the `New Order` tab on the sidebar and paste the following in the Raindex editor: 
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
  A. Limit order metrics:
    scenario: limit-order.buy.prod
    metrics:
      - label: Maximum USDC offered
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
Alternatively, you can also click on the `Load` button present in the top right corner of the editor and load the `limit-order.rain` file from your local machine if you have saved it before.

<img src="/img/raindex/raindex_load_file.png" />  

## Connect wallet
Next, let's connect the wallet to the Raindex app. You can connect your wallet by clicking the `Connect to Wallet` button in the sidebar; a modal will pop up giving you the options to connect a Ledger wallet or connect via WalletConnect.
<img src="/img/raindex/raindex_connect_wallet.png" />  

- If you have a Ledger wallet, you can select the `Ledger Wallet` button from the modal. Enter the correct derivation index for the Ledger wallet and click on the `Connect` button.
<img src="/img/raindex/raindex_ledger_wallet.png" />  

- Alternatively, you can connect your wallet via `WalletConnect`. Click on the `WalletConnect` option from the modal, click on the `Connect` button, and scan the WalletConnect QR code from your wallet to connect.
<img src="/img/raindex/raindex_wallet_connect.png" />  

- Once your wallet is connected, you'll see the connected wallet's address preview on the sidebar panel.
<img src="/img/raindex/raindex_connect_preview.png" />  
   
- Finally, open the `Settings` tab again and add the `accounts` top-level element and paste your wallet address under an identifier key. For example: 
```yaml
accounts:
  mobile-wallet: 0xCA2Dac87aD301250D7D69f50Cf0890Bc5d6Ca994
```
<img src="/img/raindex/raindex_settings_account.png" />  

## Deployments
Each dotrain file has a `deployments` top-level element in the YAML that defines `order` (consisting of input and output vaults) and associated `scenario`. The rainlang composition for this scenario is then converted to bytecode and is passed as part of the order.
For our `limit-order` example, the bytecode for composed `limit-order.buy.prod` scenario will be part of the `base-weth-buy` along with the input and output vaults of the order. 
```
deployments:
  base-weth-buy:
    scenario: limit-order.buy.prod
    order: base-weth-buy
```
After connecting your wallet, scroll to the bottom of the editor and select the deployment to be submitted and click on the `Add Order` button. 
<img src="/img/raindex/raindex_add_order.png" />  

A modal will pop up; click on the `Add Order` button on the prompt and confirm the transaction in your wallet.
<img src="/img/raindex/raindex_confirm_add_order.png" />  
 
Wait for the order to be confirmed; the order is now deployed!
