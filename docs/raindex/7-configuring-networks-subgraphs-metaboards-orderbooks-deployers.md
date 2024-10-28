# Configuring networks, subgraphs, metaboards, orderbooks and deployers

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

### Optional Fields
- `network-id`: Network identifier (usually matches chain-id)
- `currency`: Native currency symbol

## 2. Subgraphs
Subgraphs provide indexing services for orderbooks, currently maintaining a 1:1 relationship with orderbook configurations.

### Configuration Structure
```yaml
subgraphs:
  <network-key>: <subgraph-endpoint>
```
- Currently the subgraphs map 1:1 directly to the orderbooks, each orderbook has its own subgraph. 
- The GraphQL endpoint is used for fetching data indexed by the graphql server and also provides a query interface for orderbook data.

## 3. Metaboards
- Metaboard is an onchain contract for posting Rain meta about a subject (an address). This meta is indexed by a metaboard subgraph. Currently these are 1-1 with networks.

### Configuration Structure
```yaml
metaboards:
  <network-key>: <metaboard-subgraph-endpoint>
```
- Indexes the rain meta associated with the subject, the associated meta is available as aid with writing rainlang in the `Words` tab of the raindex editor

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
- References network configuration via network key
- Links to corresponding subgraph for indexing
- Manages order execution and matching

## 5. Deployers
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

## Best Practices

### Configuration Management
1. **Centralized Settings**
   - Store common settings in application configuration, have the rain documents partitioned within folders for specific settings.
   - Avoid duplicating settings across strategies, common settings can simply be added in the raindex app under the `Settings` section.
   

  <img src="/img/raindex_settings.png" />  


2. **Network References**
   - Use consistent network keys across configurations
   - Document network-specific requirements
   - Validate RPC endpoints before deployment

3. **Contract Addresses**
   - Maintain address registry for each network, this can simply be the settings file thats associated with the raindex app.
   - Document contract deployment information along with the necessary metadata (transaction hash, block number, network).

### Validation
- Verify network configurations against chainlist.org, validating the rpc-endpoints, network and chain ids, native currency etc
- Test subgraph endpoints for availability, the subgraph health can be checked by simply pinging the endpoint.
- Confirm that the deployed contract addresses sit under the correct section within the document, `deployers` section should only have deployer contracts, `orderbooks` section should only have orderbook contracts.
- Validate metaboard indexing by navigating to the `Words` tab in the editor.
