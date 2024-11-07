# Setting orders, vaults and tokens
- We can configure `tokens` and `orders` associated with the rainlang in yaml which are the top level elements in the front matter.

## Token Configuration
Tokens represent ERC20 contracts that your strategy will interact with. Each token configuration maps a name-identifier to specific contract details.

### Configuration Structure
```yaml
tokens:
  <token-identifier>:
    network: <network-reference>
    address: <contract-address>
```

### Required feilds
- `network`: Reference to a configured network
- `address`: ERC20 contract address

### Optional feilds
- `decimals` (fetched from contract)
- `label` (fetched from contract, called name in the erc20 interface)
- `symbol` (fetched from contract)
- Note that even if the yaml structure mentions that `decimals`, `symbol` are optional feilds, they are not mandated by the ERC20 standard. If the `decimals` and `symbol` feild are not provided the raindex app fetches the token symbol and decimal by quering the on-chain token contract address. If the contract query fails in case of some tokens where these methods are implemented, then this MUST be treated as an error for user to fix by populating these feild explicitly. Note the rpc url used to query contract info is the same rpc-url fetched from the settings under the `network` key.

### Example
- Following our example for the `limit-order` strategy, the token decimals and symbols for `WETH` and `USDC` are fetched by quering the token contracts on base network.
```
tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
  base-usdc:
    network: base
    address: 0xA72e7279157b9840e5B4b911d142416F80fcDf07
```
- Alternatively token names and symbols can be mentioned explicity. 
```
tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
    decimals: 18
    label: Wrapped Ether
    symbol: WETH
  base-usdc:
    network: base
    address: 0xA72e7279157b9840e5B4b911d142416F80fcDf07
    decimals: 6
    label: USDC
    symbol: USDC
  
```
### Best Practices
**Naming Conventions**
  - Use descriptive identifiers, include network prefix for clarity (e.g., `base-weth`, `arbitrum-usdc`)
  - Maintain consistent naming patterns

**Address Validation**
  - Verify contract addresses on block explorers along with the token names, symbol and decimals.
  - Ensure addresses are checksummed to avoid checksum fails.
  - Add the optional symbol and decimals to the document.


## Vault System

### Overview
Vaults are user-specific token containers that manage balances within Raindex strategies. Specifying vaults allows us to build strategies that rotate tokens between different vaults. 

A Vault ID can be any numerical hex value ranging from `0` to `115792089237316195423570985008687907853269984665640564039457584007913129639935`.

Raindex automatically creates vaults if they are not specified. Users can specify vaults in a dotrain file. 

## Order Configuration

### Overview
Orders define the input and output vaults associated with the trading strategy.

### Configuration Structure
```yaml
orders:
  <order-identifier>:
    orderbook: <orderbook-reference>
    inputs:
      - token: <token-reference>
        vault-id: <vault-identifier>
    outputs:
      - token: <token-reference>
        vault-id: <vault-identifier>
```
### Required feilds
- `inputs`: Input token vaults associated with the order
- `outputs`: Output token vaults associated with the order
- `orderbook`: Orderbook identifier to add the add to

### Optional feilds
- `deployer` - Optional deployer reference. If not specified defaults to the deployer referenced by the network for the specified orderbook

### Implementation Examples
Following our example for `limit-order`, we can see that the order has `WETH` token vault as input vault and `USDC` token vault as output token vault, which will be added under  orderbook resolved by `base` key in the `orderbooks` section of the `Settings` tab.
```
orders:
  base-weth-buy:
    orderbook: base
    inputs:
      - token: base-weth
        vault-id: 0x4767d92a5f01500424d2a2dd88964314f8a98a6b66bcf1db362b0ad9006c93e8
    outputs:
      - token: base-usdc
        vault-id: 0x4767d92a5f01500424d2a2dd88964314f8a98a6b66bcf1db362b0ad9006c93e8
```
Also for the above order the vault-ids are explicitly mentioned, raindex automatically creates and assigns random vault-ids if they are not specified.
```
orders:
  base-weth-buy:
    orderbook: base
    inputs:
      - token: base-weth
    outputs:
      - token: base-usdc
```
Multiple input and output vaults can also be associated with an order, example : 
```
orders:
  base-weth-buy:
    orderbook: base
    inputs:
      - token: base-usdc
      - token: base-usdt
    outputs:
      - token: base-dai
      - token: base-frax
```
Multiple orders can also share vaults. Eg: The following two orders are for buying and selling weth. Both order share the same two vaults, one vault is `WETH` token vault with vault-id `342123121` and another is the `USDC` token vault with the id `342123121`. For the `base-weth-buy` USDC vault is the output vault and WETH in the input vault, where as for `base-weth-sell` USDC is input vault and WETH is the input vault.
```
orders:
  base-weth-buy:
    orderbook: base
    inputs:
      - token: base-weth
        vault-id: 342123121
    outputs:
      - token: base-usdc
        vault-id: 342123121
  base-weth-sell:
    orderbook: base
    inputs:
      - token: base-usdc
        vault-id: 342123121
    outputs:
      - token: base-weth
        vault-id: 342123121
```
### Key Components
**Order Identifier** : Order identifiers are unique names for the order, the identifier should describe what the order is about so that its known during deployment . Eg : `base-weth-buy`, `bsc-bnb-sell`. Order keys are used as reference in the deployment section. 

**Orderbook Reference** : Every order is tied with a specific orderbook, the specified orderbook identifier acts as a foriegn key in the `orderbooks` section. The assoicated orderbook, the network and the deployer associted with it determines how the rainlang will be parsed and deployed on chain.

**Input Configuration**: Specifies the input vaults which represents the tokens received by the order, where every vault is uniquely identified per user, per chain, per vault id. Multiple input vaults can be associated with a particular order.

**Output Configuration**: Specifies the output vaults which represents the tokens offered by the order, where every vault is uniquely identified per user, per chain, per vault id. Multiple output vaults can be associated with an order.
