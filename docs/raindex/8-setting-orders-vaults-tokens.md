# Setting orders, vaults and tokens

## Token Configuration
Tokens represent ERC20 contracts that your strategy will interact with. Each token configuration maps a friendly name to specific contract details.

### Configuration Structure
```yaml
tokens:
  <token-identifier>:
    network: <network-reference>
    address: <contract-address>
```

### Key Components
- `token-identifier`: Unique identifier for referencing the token
- `network`: Reference to a configured network
- `address`: ERC20 contract address

### Best Practices
1. **Naming Conventions**
   - Use descriptive identifiers (e.g., `base-weth`, `arbitrum-usdc`)
   - Include network prefix for clarity
   - Maintain consistent naming patterns

2. **Address Validation**
   - Verify contract addresses on block explorers
   - Ensure addresses are checksummed
   - Document token decimals and symbols

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

A Vault ID can be any numerical value ranging from `0` to `115792089237316195423570985008687907853269984665640564039457584007913129639935`.

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
   - Unique name for the order
   - Used for referencing in strategies

2. **Orderbook Reference**
   - Links to configured orderbook
   - Determines execution environment

3. **Input Configuration**
   - Specifies received tokens
   - Defines input vault IDs
   - Multiple inputs supported

4. **Output Configuration**
   - Specifies offered tokens
   - Defines output vault IDs
   - Multiple outputs supported
