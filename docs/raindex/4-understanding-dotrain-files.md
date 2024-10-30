# Understanding dotrain files

- A `.rain` file is a configuration format combining YAML front matter with Rainlang fragments to define trading strategies.
- `.rain` files serve as a container for Rainlang to be shared and audited simply in a permissionless and adversarial environment such as a public blockchain.
- Refer [official dotrain specification](https://github.com/rainlanguage/specs/blob/main/dotrain.md) for more details.

### File Structure
A dotrain file consists of two main sections:

* YAML Front Matter: Configuring bindings and settings
* Rainlang Fragment: Expression representing the trading strategy written in rainlang.

These sections are separated by a triple-dash delimiter (`---`). The front matter and the rainlang fragement is then composed by the tooling and the resultant composition is deployed on chain.

###  Components

The following components are typically managed through the raindex app settings and don't need to be included in every dotrain file:

* networks: Network configurations (RPC, chain ID, etc.)
* subgraphs: GraphQL endpoint configurations
* metaboards: Metaboard endpoint configurations
* orderbooks: Orderbook contract addresses and networks
* deployers: Deployment contract configurations

The following components are typically managed through a dotrain file:

* tokens: Specifying the ERC20 token addresses that are relevant to the order.
* orders: Defines the order configurations including inputs and outputs
* scenarios: Specifies different execution scenarios (testing, plottting, production).
* charts: Visualizes order metrics and simulations
* deployment: Specifies deployment order and the corresponding scenrios.

###  Creating a dotrain( `.rain` ) file

The code below is a limit order that buys `WETH` in exchange for `USDC` on the base network. 
- Note : The following rainlang startegy only serves as an example and is **NOT TO BE USED FOR TRADING REAL ASSETS**.

```
# Buy Weth : NOT TO BE USED FOR TRADING REAL ASSETS

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
- Lets create a `.rain` file for the above strategy. Once you download the raindex app, open it and navigate to the `New Order` tab from the side bar and paste the above contents in the raindex editor.

<img src="/img/raindex_editor.png" />

- Click the `Save As` button on the top right hand side of the editor, choose an appropriate location and save the file as `limit-order.rain`. The file will be saved on your local machine

  <img src="/img/raindex_save_file.png" />

- You can now reload the file from your local machine, on the top right hand side click the `Load` button and chose the file you want to load into the editor. The corresponding file will be loaded into the editor.

  <img src="/img/raindex_load_file.png" />
