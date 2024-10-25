# Setting Orders, Vaults, Tokens binding.

## Tokens
- [Tokens](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#tokens): `tokens` specify the ERC20 token `address` and the `network` for the rainlang strategy.
```
tokens:
  base-weth:
    network: base
    address: 0x4200000000000000000000000000000000000006
  base-usdc:
    network: base
    address: 0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913
```
## Orders
- [Order, Vaults](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-orders): The orders defines the raindex order and the associated input and output vaults. Vaults in raindex hold token balances and are unique to every user, per token, per vault-id. The `outputs` represents the token vaults which are offered by the order(USDC token), the `inputs` are token vaults which hold balances for the input token received by the order(WETH token). Vault ids can be any numerical value assigned ranging from `0` to `115792089237316195423570985008687907853269984665640564039457584007913129639935`. Finally the order added under the specified `orderbook`.
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
