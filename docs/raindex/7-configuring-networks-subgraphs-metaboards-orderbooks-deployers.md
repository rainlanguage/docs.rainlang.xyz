# Configuring the order book yaml
- The YAML front matter in a `.rain` file is a structured configuration section placed at the beginning of the file. It defines all the necessary settings, parameters, and metadata that the Rainlang code fragments will utilize. This front matter is crucial for composing, deploying, and auditing the trading stratgies written in rainlang.  
- [Orderbook YAML specification](https://github.com/rainlanguage/specs/blob/c09eec2045470ece7be21a8a54452d18d0cf2b63/ob-yaml.md) provides guidelines and configuration options for setting up and managing the YAML metadata.

## Networks
Networks define the blockchains where your strategies operate. Each dotrain can specify one or more networks. Every deployment requires a network to deploy the strategy. Network configuration follows the [chainlist.org](https://chainlist.org) specification.

### Configuration Structure
```yaml
networks:
  <network-key>:
    rpc: <RPC-endpoint>
    chain-id: <chain-identifier>
    network-id: <network-identifier>
    currency: <native-currency>
```

### Required Fields
- `rpc`: Network RPC endpoint URL
- `chain-id`: Unique chain identifier

### Optional Fields
- `network-id`: Network identifier (usually matches chain-id)
- `currency`: Native currency symbol

## Subgraphs
A subgraph is a tool that organizes and indexes blockchain data to make it easier to search, analyze, and display within applications. It acts like a filter, pulling only relevant information from the blockchain (such as transactions related to a specific orderbook) and presenting it in a structured way. This makes accessing specific blockchain data faster and more manageable.

### Configuration Structure
```yaml
subgraphs:
  <network-key>: <subgraph-endpoint>
```
* Currently the subgraphs map 1:1 directly to the orderbooks
* This means each orderbook has its own subgraph. 
* The GraphQL endpoint is used for fetching data indexed by the graphql server and also provides a query interface for orderbook data.

## Metaboards
- A Metaboard is a smart contract deployed on a blockchain network that allows users to post metadata (referred to as "Rain meta") about a specific subject, typically an address on the blockchain, and metaboards are subgraphs that index the rain meta. 
- Generally all the supported networks have the metaboard contracts and the corresponding subgraphs deployed. Metaboard indexes the rain meta associated with the subject, the associated meta is available as aid with writing rainlang in the `Words` tab of the raindex editor.

<img src="/img/raindex/raindex_words.png" />  
 

### Configuration Structure
```yaml
metaboards:
  <network-key>: <metaboard-subgraph-endpoint>
```

## Orderbooks
Orderbook is the smart contract deployed on the network where bytecode representing the trading strategy written in rainlang can be added as a part of the order along with the associated input and output token vaults.

### Configuration Structure
```yaml
orderbooks:
  <orderbook-key>:
    address: <contract-address>
    network: <network-key>
    subgraph: <subgraph-key>
```
- `address`: Orderbook contract address
- `network`: References `networks` configuration via network key, default network is same as `orderbook-key`
- `subgraph`: References `subgraphs` configuration via subgraph key, default is same as `orderbook-key`

## Deployers
Deployer contracts serve as entry points for DIS (Deployer Interpreter Store) pairs.

### Configuration Structure
```yaml
deployers:
  <network-key>:
    address: <deployer-contract-address>
    network: <network-reference>
```
- Acts as logical entry point for DIS pairs
- Network inheritance from key name if not specified

## Example
Referring to our `limit-order` example, we can simply put the `networks`, `subgraphs`, `metaboards`, `orderbooks` and `deployers` from the rain documents into the `Settings` which can be accessible throught the app. Navigate to the settings tab, and paste the following settings in the tab.
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

 <img src="/img/raindex/raindex_settings.png" />  

Once pasted, the settings will be applied throught the app and can be accessed with the parameter keys. Since we already have the settings loaded, we can remove the above yaml section from our rain document. Remove `networks`, `subgraphs`, `metaboards`, `orderbooks` and `deployers` from `limit-order.rain` file, it should look something like this : 
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
Notice that the above document does not contain the yaml present in the settings tab, however the same settings are directly applied into the rain document within the editor. Also note that the `network`, `orderbook` and `deployer` mentioned in the document uses the same keys used under settings tab. 
Eg: 
```
tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
  base-usdc:
    network: base
    address: 0xA72e7279157b9840e5B4b911d142416F80fcDf07
```
- For the above `tokens` section, the `network: base` resolves to `base` network under the `networks` section in the `Settings` tab. 
## Best Practices

### Configuration Management
**Centralized Settings :** Store common settings in application configuration, have the rain documents partitioned within folders for specific settings. Avoid duplicating settings across strategies, common settings can simply be added in the raindex app under the `Settings` section.
   
**Network References :** Use consistent network keys across configurations, document network-specific requirements.Validate RPC endpoints for networks before deployment

**Contract Addresses :** Maintain address registry for each network, this can simply be the settings file thats associated with the raindex app. Document contract deployment information along with the necessary metadata (transaction hash, block number, network).

### Validation
- Verify network configurations against chainlist.org, validating the rpc-endpoints, network and chain ids, native currency etc
- Test subgraph endpoints for availability, the subgraph health can be checked by simply pinging the endpoint.
- Confirm that the deployed contract addresses sit under the correct section within the document, `deployers` section should only have deployer contracts, `orderbooks` section should only have orderbook contracts.
- Validate metaboard indexing by navigating to the `Words` tab in the editor.

