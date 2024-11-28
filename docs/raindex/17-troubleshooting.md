# Troubleshooting

## Common Issues
1. **Network Connectivity**: Ensure that the network RPC URLs mentioned under the `networks` top-level element in the `Settings` tab are connected to valid nodes; the RPC URL should also be indexing latest blocks. Services like [Infura](https://www.infura.io/) and [Alchemy](https://www.alchemy.com/) provide free RPC endpoints for almost all major networks, with ready-made dashboards for monitoring availability, health, synchronization, etc. The user must also take explicit care in using `key: value` pairs for YAML that are meaningful so as to avoid network mismatch. For example:
```
networks:
   ethereum-mainnet: 
      rpc: https://mainnet.base.org
```
Here the RPC URL resolves to Base network mainnet, whereas the key indicates it is for Ethereum mainnet, which is incorrect.
Note that while doing simulation runs or backtesting, there can be a large volume of requests being made to RPC URL for that network depending upon the `runs` parameter that is set for that network. For certain cases of backtesting, it may be required to use a full node RPC to access historical block data.

2. **Subgraph Problems**: Raindex provides subgraph endpoints for all the contracts across all the supported networks. The subgraphs code is open source; anybody can review, edit, and deploy their own subgraphs. However, the open-source nature of the infrastructure implies that the subgraphs should be monitored for delays in indexing block data for the subgraph, failure to resolve queries, schema mismatches, etc., by the user beforehand.

3. **Contract Integration**: Raindex also provides all the deployment contract addresses for all the core infrastructure contracts, for all supported networks. Users must ensure that all the contracts that are present in the YAML front matter, bindings, settings, etc., are valid and present under the correct identifier for that network. One can simply view the contract on the block explorer for that network to check. All the contract source code is open-sourced and maintained under the official [Rainlang GitHub repository](https://github.com/rainlanguage/); anybody can deploy their own smart contracts to work on their private infrastructure managing their own liquidity.

## Resolution Steps
1. Verify network connectivity and RPC endpoints, monitor the health, synchronization status, and number of RPC requests.
2. Check subgraph synchronization status by pinging the GraphQL endpoint, ensure that the block data is indexed and mapped to the schemas correctly.
3. Ensure that all the contract addresses present in the YAML front matter, settings, bindings, etc., are valid by verifying them on the block explorer.

## Security Considerations

### Network Security
Public RPC endpoints receive large volumes of requests, often get rate-limited, and may not be in sync with the latest block height. It is recommended to use private RPC endpoints for all networks for running simulations and deployments; avoid sharing the settings which have private endpoints so that only you can access them. For networks that incur high transaction costs like Ethereum mainnet, it is recommended to use Flashbots RPC which provides MEV protection, gas and MEV refunds, and faster inclusion of transactions.

### Contract Security
All Rainlanguage contracts are open source and can be verified and audited by any external participant. Check the deployment contract addresses by checking them on block explorers; any contracts which are not present under the Rain contract registry must be verified by the user.

## Development Tools
- Raindex editor available within the Raindex app can be used to simulate on-chain behavior of the Rainlang with all the given bindings contracts addresses for that network
- Services like Infura, Alchemy offer free RPC endpoints and dashboards to monitor the health of the network RPC endpoints
- Health of the subgraphs can be checked by simply querying the Graph API endpoint
- Raindex contract registry and block explorers to verify the deployment contract addresses

## See Also
- [YAML Spec](https://yaml.org/spec/)
- [Chainlist Documentation](https://chainlist.org)
- [The Graph Documentation](https://thegraph.com/docs/en/)
