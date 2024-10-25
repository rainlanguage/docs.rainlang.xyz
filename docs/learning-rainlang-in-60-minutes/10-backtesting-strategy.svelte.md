# Backtesting Limit Order
- For our example, it's useful to check what the actual on-chain ETH price is for recent historical blocks before we set `limit-ratio` binding. In the `limit-order.buy.backtest` scenario we define `runs` and specify the `block` range, runs within this context mean that for how many times the expression will be evaluated on the parituclar block and range specify the block on which it will be evaluated, with given block interval specifying the number of block to skip within successive runs. So for the block range `21530860..21531860` with a interval of `100`, the rainlang expression will be evaluated for blocks 21530860, 21530960, 21531060 and so on till the block 21531860.
```
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
For plotting, we plot the `block-number` on X-axis and the `current-price` on Y-axis.
<img src="/img/backtest.png" />