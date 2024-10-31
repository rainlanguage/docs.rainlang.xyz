# Backtesting

## Overview
Backtesting allows developers to evaluate Rainlang expressions against historical blockchain data. Backtests can be used to validate the trading strategy against all the historical prices of the asset. The performance of the strategy can be analyzed accross different market conditions and time periods by evaluting the rainlang expression at those particular blocks.                                          

## Historical Evaluation
Expressions are evaluated against specific historical blocks. Each evaluation provides insights into how the strategy would have performed at that point in time, at that specific block and results can be visualized through plots and metrics.

## State Management
**Important:** Contract storage states are not persisted between successive runs in the block range. Each evaluation is independent and starts with fresh state.

# Implementation Details

## Block Range Configuration
Backtesting is configured using following parameters:

1. **Block Range**: Specified as `startBlock..endBlock`
   ```yaml
   # Example: 21530860..21531860
   ```

2. **Block Interval**: Number of blocks to skip between evaluations
   ```yaml
   # Example: interval of 100 means evaluating blocks:
   # 21530860, 21530960, 21531060, ...
   ```

3. **Runs**: Number of times to evaluate the expression for each block

- Refer [front matter scenarios](
https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios) for the official specification.

## Example: Backtesting a Limit Order

The following configuration demonstrates how to backtest a limit order strategy:

```yaml
C. Limit order backtest:
      scenario: limit-order.buy.backtest
      plots:
        Buy WETH backtest:     
          x:
              label: 'Block Number'
          y:
              label: 'WETH/USDC Ratio'
          marks:
            - type: line
              options:
                x: 0.1
                y: 0.0  
```
<img src="/img/raindex/raindex_backtest.png" />  

## Best Practices

### Selecting Block Ranges
1. Choose historically significant periods
2. Consider market volatility periods
3. Test across different market conditions

### Interval Selection
Smaller intervals provide more granular data but increase computation time, larger intervals are more efficient but may miss short-term price movements. Consider the time horizon for which the startegy will be active, it could be a week, a month or a year. If the strategy is going to be short lived, then block range interval can be small, else for long standing strategies block ranges can be larger.

### Performance Considerations
Backtesting involves quering the blocks for historical data by making rpc call. Wider range and shorter intervals of the blocks implies that more number of rpc calls will be made. This may result in backtesting simulation requiring some time to complete the runs, also if the block range is too wide a large number of rpc requests will be made to the rpc provider which could end up rate limiting the endpoint. There may be a requirement to use a full node provider in case where data from older blocks need to be accessed.

## Common Use Cases

1. **Price Analysis** :
Back testing is useful for tracking historical price movements of an asset to identify entry/exit points and to judge the appropriate binding values for the strategy parameters.

2. **Strategy Validation** : 
Get feedback on expression evaluation for the current instance of the parameters and iteratively modify and validate them by checking the order amounts and ratios.

3. **Risk Assessment** :
Analyze the behaviour of the strategy during adversarial market condition and periods of high market volatility to identify potential edge cases for the strategy and mitigate the risk by adding guardrails within the strategy itself.

## Limitations and Considerations

1. **State Persistence** : Storage states within historical blocks are not persisted between runs, each evaluation starts fresh.

2. **Historical Data Availability** : Some older blocks might not be accessible depending upon the provided node rpc url, if a full node rpc isn't available strategy cannot be tested for all the historical data.

3. **Performance** : Large block ranges with small intervals may impact performance will executing backtest, with large number of rpc calls made to the provider which may contributes to rpc rate limit and results is longer time for backtesting simulation to run.
   