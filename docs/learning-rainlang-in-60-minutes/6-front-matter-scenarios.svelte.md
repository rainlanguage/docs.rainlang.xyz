# Front Matter Scenarios
- [Scenarios](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios): From the ob-yaml specification, _Scenarios are a hierarchical structure that specifies bindings used by dotrain tooling to produce a concrete rainlang instance_ . The yaml fragment can have multiple parent-child scenarios nested within each other, each specifying different bindings resulting in different composition for the same rainlang. Each root scenario falls under specific `network` which match the specified `deployer` and `orderbook`. 
- Here we have root scenario as `limit-order` and nested `buy` as the child scenario and the specified bindings for ratio, amount and the subparser contract address. Then we have additional nested scenarios specifying the bindings for production when the order is deployed, along with the binding for plotting charts, metrics and backtesting the rainlang strategy. 
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