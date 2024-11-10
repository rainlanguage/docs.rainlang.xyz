# Troubleshooting

## Common Issues
1. **Network Connectivity** : Ensure that the network rpc urls mentioned under the `networks` top level element in the `Settings` tab are connected to valid nodes, the rpc url should also be indexing latest blocks. Services like [infura](https://www.infura.io/) and [alchmey](https://www.alchemy.com/) provide free rpc endpoint for almost all major networks, with ready-made dashborads for monitoring availiabiltiy, health, syncronization etc. The user must also take explicity care in using `key: value` pairs for yaml are meaningful so as to avoid network mismatch. Eg : 
```
networks:
   ethereum-mainnet: 
      rpc: https://mainnet.base.org
```
Here the rpc url resolves to base network mainnet, where as the key indicates it is for ethereum mainnet, which is incorrect.
Note that while doing simulation runs or backtesting, there can be a large volume of request being made to rpc url for that network depending upon the `runs` parameter that is set for that network. For certain cases of backtesting it maybe required to use a full node rpc to access historical block data.

2. **Subgraph Problems** : Raindex provides subgrahs endpoints for all the contracts accross all the supported network. The subgraphs code is open source, anybody can review, edit and deploy their own subgraphs. However the open source nature of the infratsructure implies that the subgraphs should be monitored for delays in indexing block data for the subgraph, failure to resolve queries, sehema mismatches etc by the user beforehand. 

3. **Contract Integration** : Raindex also provides all the deployment contract addresses for all the core infrastructure contracts, for all supported networks. User must ensure that all the contract that are present in the yaml front matter, bindings, settings etc are valid and present under the correct identifier for that network. One can simply view the contract on the block explorer for that network to check. All the contract source code is open sourced and maintained under the official [rainlang github repository](https://github.com/rainlanguage/), anybody can deploy there own smart contracts to work on there private infra managing there own liquidity.  

## Resolution Steps
1. Verify network connectivity and RPC endpoints, monitor the health, syncronization status, number of rpc request.
2. Check subgraph synchronization status by pinging the graphql endpoint, ensure that the block data is indexed and mapped to the schemas correctly.
3. Ensure that all the contract addresses present in the yaml front matter, settings, bindings etc are valid by verifying them on the block explorer.

## Security Considerations

### Network Security
Public rpc endpoints receive large volumes of requests, often get rate limited and maynot be in sync with the latest block height. It is recommended to use private rpc endpoint for all networks for running simulation and deployments, avoid sharing the settings which have private endpoints so that only you can access them. For networks that incur high transaction costs like ethereum mainnet it is recommend to use flashbots rpc which provide MEV protection, gas and MEV refunds and faster inculsion of transactions.

### Contract Security
All rainlanguage contracts are open source and can be verified and audited by any external participant. Check the deployment contract addresses by checking them on block explorers, any contracts which are not present under the rain contract registry must be verified by the user.  

## Development Tools
- Raindex editor available with in the raindex app can be used to simulate on-chain behaviour of the rainlang with all the givem bindings contracts addresses for that network. 
- Services like infura, alchemy offer free rpc endpoints and dashboards to monitor the health of the network rpc endpoints. 
- Health of the subgraphs can be checked by simplying querying the graph api endpoint. 
- Raindex contract registry and block explorers to verify the deployment contract address.

## See Also
- [YAML Spec](https://yaml.org/spec/)
- [Chainlist Documentation](https://chainlist.org)
- [The Graph Documentation](https://thegraph.com/docs/en/)
