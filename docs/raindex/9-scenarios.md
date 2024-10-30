# Front Matter Scenarios
[Scenarios](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios) in dotrain files provide a hierarchical configuration structure that generates specific rainlang instances through composition for that specific scenario. This system allows developers to define multiple execution contexts for the same rainlang, enabling production deployments, testing, and simulation runs from a single configuration.

## Scenario Structure

### Basic Anatomy
```yaml
scenarios:
  <root-scenario>:
    network: <network-reference>
    deployer: <deployer-reference>
    orderbook: <orderbook-reference>
    bindings: <binding-definitions>
    scenarios: <nested-scenarios>
```
### Optional Feilds
- `network`: Resolves to network of same name as definied under `networks`, if not set root scenario only.
- `deployer`: Resolves to the deployer of the same name as defined under `deployers`, if not set root scenario only.
- `orderbook`: Resolves to the orderbook of the same name as defined under `orderbooks`, if not set root scenario only. 
- `bindings`: Bindings for this scenario level, each binding is a `key: value` pair, forwarded to dotrain tooling as strings.
-  `blocks`: A block range for the fuzzer to iterate over. 
- `entrypoint`: A custom entrypoint to use for the scenario, only charts can use this entrypoint.

- A complete scenario could look like:
```
scenarios:
  mainnet:
    bindings:
      foo: ...
      bar: ...
    scenarios:
      sell:
        bindings:
          bing: ...
      buy:
        bindings:
          bing: ...
  fuzzer:
    network: testnet
    deployer: testnet
    orderbook: testnet
    runs: 10
    blocks:
      range: [10000..]
      interval: 1000
    # bar intentionally not set so it would be fuzzed
    bindings:
      foo: ...
      bing: ...
```
- Refer the above linked dotrain specification. 
### Hierarchy Levels

1. **Root Scenario**
   - Defines base network context
   - Specifies core infrastructure references like networks, subparser contract address etc.
   - Contains global bindings that are common throughout all the child scenarios.

2. **Child Scenarios**
   - Inherit parent configurations.
   - Extend the existing parent bindings. Currently shadowing is disallowed, if a child specifies a binding that is already set by the parent, this is an error.
   - Can contain further nested scenarios.

3. **Leaf Scenarios**
   - Scenarios for specific use cases (prod, test, simulation)
   - Leaf scenarios represent the final scenarios that result in the complete composition, where all the bindings for that particular leaf scenario are resolved in a rainlang document.

## Scenario Hierarchy Example
- A single dotrain can have multiple heirarchies consisiting of multiple child and leaf scenarios.
- Consider the following nested scenarios within the `limit-order` example consisting of root, child and leaf scenrios and their assoicated bindings, let's try and understand the scenario hierarchy.
```
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
```
### Root Scenario
- `limit-order` is the root scenario, specifying the network along with deployer and the orderbook that will be used for parsing and evaluating the rainlang expression.
- Only `network` is the required feild, if not specified `orderbook` and `deployer` are resolved by referencing the same network key.
- Root scenarios contains bindings which will be inherited by all the nested scenarios, here we have the `orderbook-subparser` which basically enables us to use the rainlang words associated with the subparser contracts. Since we want all the further scenarios to inherit this binding, it is kept as a root level scenario. Also the `network`, `deployer` and `orderbook` that are associated with child scenarios are mentioned in the root scenario.

### Child Scenario
- Inherits all the bindings from the parent scenario.
- `buy` is the child scenario, specifying the bindings that will be inherited by all the leaf scenarios.
- In `buy` child scenario we have the bindings for `limit-ratio` and `output-amount` which bascially defines the order output amount and ratio for the order. Since these bindings will the used for plotting and production scenarios they are defined in the child heirarchy.

### Leaf Scenarios
- Inherits all the bindings from the child scenarios, specify use-case specific bindings resulting in different instances of the rainlang composition tied to specific use case.
- We only have one binding here left to elide and that is the `get-output-amount` call binding which dictates what the output amount will be for order.
- `prod` : Specifies bindings for the resultant production composition.
- `plot` : Specifies bindings to generate plots for the strategy.
- `backtest` : Specifyies bindings to check evaluation on historical blocks.

Refer [front matter scenarios](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios) from the offical specification for detailed explanation.