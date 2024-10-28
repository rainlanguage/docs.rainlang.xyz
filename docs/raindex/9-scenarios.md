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
- Consider the following nested scenarios within the `limit-order` example consisting of root, child and leaf scenrios and their assoicated bindings.
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
- Contains bindings which will be inherited by all the nested scenarios.

### Child Scenario
- Inherits all the bindings from the parent scenario.
- `buy` is the child scenario, specifying the bindings that will be inherited by all the leaf scenarios.

### Leaf Scenarios
- Inherits all the bindings from the child scenarios, specify use-case specific bindings resulting in different instances of the rainlang composition tied to specific use case.
- `prod` : Specifies bindings for the production composition.
- `plot` : Specifies bindings to generate plots for the strategy.
- `backtest` : Specifyies bindings to check evaluation on historical blocks.

Refer [front matter scenarios](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios) from the offical specification for detailed explanation.