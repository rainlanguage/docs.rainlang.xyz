# Configuaring networks, subgraphs, metaboards, orderbooks and derployers

## 1. Networks
Networks define the blockchains where your strategies operate. Configuration follows the [chainlist.org](https://chainlist.org) specification.

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
- `network-id`: Network identifier (usually matches chain-id)
- `currency`: Native currency symbol

## 2. Subgraphs
Subgraphs provide indexing services for orderbooks, maintaining a 1:1 relationship with orderbook configurations.

### Configuration Structure
```yaml
subgraphs:
  <network-key>: <subgraph-endpoint>
```

### Key Characteristics
- Maps directly to orderbook configurations
- Uses GraphQL endpoints for data indexing
- Provides query interface for orderbook data

## 3. Metaboards
Metaboards are on-chain contracts that store metadata about specific addresses.

### Configuration Structure
```yaml
metaboards:
  <network-key>: <metaboard-subgraph-endpoint>
```

### Important Notes
- Maintains 1:1 relationship with networks
- Indexes on-chain metadata
- Provides queryable interface for address metadata

## 4. Orderbooks
Orderbooks represent deployed orderbook contracts.

### Configuration Structure
```yaml
orderbooks:
  <network-key>:
    address: <contract-address>
    network: <network-reference>
    subgraph: <subgraph-reference>
```

### Key Features
- References network configuration via network key
- Links to corresponding subgraph for indexing
- Manages order execution and matching

## 5. Deployers
Deployer contracts serve as entry points for DIS (Decentralized Infrastructure Service) pairs.

### Configuration Structure
```yaml
deployers:
  <network-key>:
    address: <deployer-contract-address>
    network: <network-reference>
```

### Important Notes
- Acts as logical entry point for DIS pairs
- Network inheritance from key name if not specified
- Provides queryable interface for associated addresses

## Best Practices

### Configuration Management
1. **Centralized Settings**
   - Store common settings in application configuration
   - Avoid duplicating settings across strategies
   - Use version control for settings management

2. **Network References**
   - Use consistent network keys across configurations
   - Document network-specific requirements
   - Validate RPC endpoints before deployment

3. **Contract Addresses**
   - Maintain address registry for each network
   - Document contract deployment information
   - Include deployment block numbers for reference

### Validation
- Verify network configurations against chainlist.org
- Test subgraph endpoints for availability
- Confirm contract addresses are deployed and accessible
- Validate metaboard indexing functionality
