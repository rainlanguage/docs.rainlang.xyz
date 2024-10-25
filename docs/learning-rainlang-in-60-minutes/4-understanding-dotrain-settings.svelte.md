# Understanding dotrain settings
- The `networks`, `subgraphs`, `metaboards`, `orderbooks` and `delpoyers` are commonly reffered as "settings" which are unchanged for strategies across different networks and are mapped within the app itself. 
## Networks
- Networks are specified in the format defined under chainlist.org
```
networks:
  base:
    rpc: https://mainnet.base.org
    chain-id: 8453
    network-id: 8453
    currency: ETH
```
## Subgraphs
- Subgraphs which are url string for the indexers are given as 1:1 with orderbooks.
```
subgraphs:
  base: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/ob4-base/0.9/gn
```
## Metaboards
- Metaboard is an onchain contract for posting Rain meta about a subject (an address). This meta is indexed by a metaboard subgraph. Currently these are 1-1 with networks.
```
metaboards:
  base: https://api.goldsky.com/api/public/project_clv14x04y9kzi01saerx7bxpg/subgraphs/mb-base-0x59401C93/0.1/gn
```
## OrderBooks
- Every orderbook is a contract deployed on some chain (has an address) with a subgraph that knows how to index it into the form expected by the application, with the orderbook key acting as foreign key for `networks`. Eg: Here the orderbook under the key `base` will be tied to `network` and `subgraph` values with the same key. 
```
orderbooks:
  base:
    address: 0xd2938e7c9fe3597f78832ce780feb61945c377d7
```
## Deployers
- Each deployer contract serves as the logical entrypoint into an entire DISpair (the associated addresses can be queried from the deployer onchain), with the network for the deployer assumed same as the deployer name key if not set.
```
deployers:
  base:
    address: 0xC1A14cE2fd58A3A2f99deCb8eDd866204eE07f8D
    network: base
```
