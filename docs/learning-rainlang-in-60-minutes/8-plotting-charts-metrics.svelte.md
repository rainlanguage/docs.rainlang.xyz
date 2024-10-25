# Plotting Charts and Metrics
- For our document we simply display the amount and ratio offered by our order as singular scalar values. Referring to the `calculate-io` stack indexed `0`, the order amount and ratio are indexed `0.2` and `0.3` respectively. 
```
charts:
  A. Limit order metrics :
    scenario: limit-order.buy.prod
    metrics:
      - label: Maxmimum USDC offered
        value: 0.2
        unit-suffix: " USDC"
        description: 'Maximum USDC offered by the order.'
      - label: Order ratio
        value: 0.3
        unit-suffix: " WETH/USDC"            
        description: 'Order ratio at which WETH was purchased.'
```
<img src="/img/metrics.png" />
- We can also plot any particular scenario within the raindex app. We simply name the scenario to plot and then expose observable plot config directly to the yaml. Here we plot the 
`limit-order.buy.prod` scenario which has its own bindings and runs. The `runs` for the plot define the number fuzz runs, where different values for the fuzz input are generated and the expression is evaluated for each of these. This particular plot shows that even if the output amount offered by the order changes, the io-ratio remains unchanged. 
```
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
<img src="/img/plot.png" />