# Setting orders, vaults and tokens

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

### Best Practices
1. **Naming Conventions**
   - Use descriptive identifiers, include network prefix for clarity (e.g., `base-weth`, `arbitrum-usdc`)
   - Maintain consistent naming patterns

2. **Address Validation**
   - Verify contract addresses on block explorers along with the token names, symbol and decimals.
   - Ensure addresses are checksummed to avoid checksum fails.
   - Add the optional symbol and decimals to the document.

## Implementation Examples

### Basic Token Setup
```yaml
tokens:
  mainnet-eth:
    network: ethereum
    address: 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2
```

### Multi-Token Order
```yaml
orders:
  eth-usdc-swap:
    orderbook: mainnet
    inputs:
      - token: mainnet-eth
        vault-id: 0x1234...
    outputs:
      - token: mainnet-usdc
        vault-id: 0x5678...
```

## Vault System

### Overview
Vaults are user-specific token containers that manage balances within Raindex strategies. Specifying vaults allows us to build strategies that rotate tokens between different vaults. 

A Vault ID can be any numerical hex value ranging from `0` to `115792089237316195423570985008687907853269984665640564039457584007913129639935`.

Raindex automatically creates vaults if they are not specified. Users can specify vaults in a dotrain file. 

## Order Configuration

### Overview
Orders define trading parameters and vault associations for automated trading strategies.

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

### Key Components
1. **Order Identifier**
   - Unique name for the order, the identifier should describe what the order is about so that its known during deployment . Eg : `base-weth-buy`, `bsc-bnb-sell`.
   - Order keys are used as reference in the deployment section. 

2. **Orderbook Reference**
   - Every order is tied with a specific orderbook, the specified orderbook identifier acts as a foriegn key in the `orderbooks` section. 
   - The assoicated orderbook, the network and the deployer associted with it determines how the rainlang will be parsed and deployed on chain.

3. **Input Configuration**
   - Specifies the input vaults which represents the tokens received by the order, where every vault is uniquely identified per user, per chain, per vault id.
   - Multiple input vaults can be associated with a particular order.

4. **Output Configuration**
   - Specifies the output vaults which represents the tokens offered by the order, where every vault is uniquely identified per user, per chain, per vault id.
   - Multiple output vaults can be associated with an order.
