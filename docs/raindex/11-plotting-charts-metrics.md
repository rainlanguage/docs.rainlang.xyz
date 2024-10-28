# Plotting Charts and Metrics

## Overview
Rain Protocol supports visualization of scenarios through charts and metrics. Each concrete set of bindings can be treated as a data point, enabling both single-point and multi-point (fuzzing) visualizations.

## Metrics
Metrics represent single scalar values displayed with contextual information. Each metric requires at minimum a `label` and `value`, with optional additional fields for enhanced context.

### Required Fields
- `label`: Display name for the metric
- `value`: Numerical value to display

### Optional Fields
- `unit-suffix`: Unit of measurement to append
- `description`: Detailed explanation of the metric

(Refer [front matter charts](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-charts))
### Example: Limit Order Metrics
The following example displays the amount and ratio offered by an order as scalar values, referencing the `calculate-io` stack:

```yaml
charts:
  A. Limit order metrics:
    scenario: limit-order.buy.prod
    metrics:
      - label: Maximum USDC offered
        value: 0.2
        unit-suffix: " USDC"
        description: 'Maximum USDC offered by the order.'
      - label: Order ratio
        value: 0.3
        unit-suffix: " WETH/USDC"            
        description: 'Order ratio at which WETH was purchased.'
```
<img src="/img/raindex_metrics.png" /> 
## Plots
Plots provide visualization of scenarios with x/y axes, supporting both single points and multiple data points through fuzzing.

### Key Components
1. **Scenario Definition**: Specify which scenario to plot
2. **Axis Configuration**: Define labels and settings for x and y axes
3. **Mark Types**: Specify how data should be visualized (e.g., lines, points)

### Example: Limit Order Simulation
This example demonstrates plotting a limit order simulation that shows the relationship between output amount and IO ratio:

```yaml
B. Limit order simulation:
    scenario: limit-order.buy.plot
    plots:
      Buy WETH simulation:     
        x:
            label: 'USDC Output'
        y:
            label: 'WETH/USDC Ratio'
        marks:
          - type: line
            options:
              x: 0.2
              y: 0.3 
```
<img src="/img/raindex_plot.png" /> 

- When using fuzzing, the `runs` parameter determines the number of fuzz iterations
- Each fuzz run generates different values for the fuzz input variable and evaluates the expression

## Reference
For detailed specifications, see the [dotrain specification](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-charts).
