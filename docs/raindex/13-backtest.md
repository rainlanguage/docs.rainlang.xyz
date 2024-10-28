# Backtesting

## Overview
Backtesting allows developers to evaluate Rainlang expressions against historical blockchain data. This feature is particularly valuable for:
- Validating trading strategies against historical price data
- Testing expression behavior across different market conditions
- Analyzing strategy performance over specific time periods

## Core Concepts

### Historical Evaluation
- Expressions are evaluated against specific historical blocks
- Each evaluation provides insights into how the strategy would have performed at that point in time
- Results can be visualized through plots and metrics

### State Management
**Important:** Contract storage states are not persisted between successive runs in the block range. Each evaluation is independent and starts with fresh state.

## Implementation Details

### Block Range Configuration
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

### Example: Backtesting a Limit Order

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

### Visualization Components
- **X-Axis**: Represents block numbers
- **Y-Axis**: Shows the current price (WETH/USDC ratio)
- **Line Plot**: Displays price evolution across the selected block range

## Best Practices

### Selecting Block Ranges
1. Choose historically significant periods
2. Consider market volatility periods
3. Test across different market conditions

### Interval Selection
- Smaller intervals provide more granular data but increase computation time
- Larger intervals are more efficient but may miss short-term price movements
- Choose based on your strategy's time horizon

### Performance Considerations
- Balance between range size and interval
- Consider computational resources
- Monitor gas costs if implementing similar logic on-chain

## Common Use Cases

1. **Price Analysis**
   - Track historical price movements
   - Identify optimal entry/exit points

2. **Strategy Validation**
   - Test limit order parameters
   - Validate price ratio assumptions

3. **Risk Assessment**
   - Analyze behavior during market volatility
   - Identify potential edge cases

## Limitations and Considerations

1. **State Persistence**
   - Storage states don't persist between runs
   - Each evaluation starts fresh

2. **Historical Data Availability**
   - Dependent on blockchain data availability
   - Some older blocks might not be accessible depending upon the provided node rpc url

3. **Performance**
   - Large block ranges with small intervals may impact performance will executing backtest
   - Consider optimizing range and interval settings
