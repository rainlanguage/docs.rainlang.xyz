# Navigating a ".rain" file

Let's start by understanding a very simple `.rain` file a break it down into smaller parts. Let's create a new file and call it `limit-order.rain` and populate the file contains with the following strategy written in rainlang that allows us to buy `WETH` in exchange for USDC on base network: 
```
raindex-version : db14c87f012a76980661802ff424371d6e84552e

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

tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
  base-usdc:
    network: base
    address: 0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913

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
- The entire file is broadly divided into two sections - the yaml front matter and the rainlang fragment, separated by the `---` dellimeter. Theses fragment are composed to a rainlang document by the composition done by the tooling available via the raindex app.
- Although the complete dotrain document includes all of the above mentioned keys present in the yaml structure, the `networks`, `subgraphs`, `metaboards`, `orderbooks` and `delpoyers` generally reamin common throughout dotrain documents, and are available as `settings` in the raindex app. Hence it is not uncommon to see dotrain documents without these settings are raindex tooling binds these elements during composition and deployment. 