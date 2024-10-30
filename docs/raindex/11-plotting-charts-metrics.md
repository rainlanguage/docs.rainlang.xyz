# Plotting Charts and Metrics

## Overview
Rain Protocol supports visualization of scenarios through charts and metrics. Each concrete set of bindings can be treated as a data point, enabling both single-point and multi-point (fuzzing) visualizations.

## Metrics
- Metrics represent single scalar values displayed with contextual information. Each metric requires at minimum a `label` and `value`, with optional additional fields for enhanced context.
- Metrics are useful for displaying values for key paramters like the order ratio or amount for faster visualization.
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
- Firstly we `charts` top level element, any scenario can be charted as every concrete set of bindings can be treated as a data point. For a single concrete set, a single data point is produced, for a fuzzer or similar, a set of data points will be produced.
- Next we give the scenario key-value pairs for which the concrete bindings are generated, in the case we want to generate metric for `limit-order.buy.prod` scenario. Next we mention the stack position of the element that we want to plot as `value` along with an appropriate label name. In our example the `final-amount` and the `final-ratio` are present on stack indexed positions `2` and `3` of the calculate stack. Optionally we can also mention the `unit-suffix` and `description` for the metrics

<img src="/img/raindex_metrics.png" /> 

- 
## Plots
- Plots provide visualization of scenarios with x/y axes, supporting both single points and multiple data points through fuzzing, by iteratively evaluating the sources for different fuzz inputs
- The plotting functionality leverages the Observable Plot library—a declarative plotting system created by the authors of D3.js. This library allows you to create a wide range of charts using simple, declarative configurations.

### Required Feild
- `<key>`: Key named plots, need at least 1 for a chart

### Optional Chart Feild
- `scenario`: Name of the scenario to draw data from, falls back to the name of the chart if not set
- `plots`:  Mapping of charts to create

#### Required fields for each mapping under plots:
- `marks`: A list of marks to draw in the plot according to the observable lib

#### Required fields for each mapping under marks:
- `type`: Can be `line`, `dot` or `recty` as per observable
- `options`: May be keys `x` and `y` that specify a stack path to a value to plot, or a `transform`

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
- Firstly we have the chart key named `B. Limit order simulation` add the associated `limit-order.buy.plot` scenario for which we want to generate the plot. 
- Next we have the `plots` mapping, the first key under plots is named `Buy WETH simulation`, the x and y axes are labelled. Then we have the required `marks` feild which we give the `type` as `line` because we want the line chart. Lastly we assign the stack elements `0.2` and `0.3` which are the `final-amount` and `final-ratio` in our `calculate-io` stack as co-ordinated to be plotted on X and Y axis.

<img src="/img/raindex_plot.png" /> 

- When using fuzzing, the `runs` parameter determines the number of fuzz iterations
- Each fuzz run generates different values for the fuzz input variable and evaluates the expression

### Visualization Examples for the tranche strategy
- One of the examples of visualization can be found for the tranche strategy under [pubstrats](https://github.com/rainlanguage/rain.dex.pubstrats/blob/main/src/tranche/tranche-space.rain). Here are some examples for metrics and charts : 

<img src="/img/raindex_tranche_metrics.png" /> 

<img src="/img/raindex_tranche_sim1.png" /> 

<img src="/img/raindex_tranche_sim2.png" /> 

<img src="/img/raindex_tranche_sim3.png" /> 

<img src="/img/raindex_tranche_sim4.png" /> 

- Here is how a histogram would look like : 

<img src="/img/raindex_histogram.png" /> 

- Here is how a hex bin plot would look like : 
<img src="/img/raindex_hex_bin.png" /> 


## Reference
For detailed specifications, see the [dotrain specification](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-charts).
