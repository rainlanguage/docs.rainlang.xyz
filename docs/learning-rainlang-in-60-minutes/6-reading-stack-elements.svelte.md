# Reading Stack Elements
- Rainlang is a stack based interpreted language, and it is possible to read the state of the stack during eval and execution. In order to leverage the features such as charting and backtesting that are available within the raindex app, the position of the elements on the stack can be easily know by looking at the resultant composition.
- In the `Rainlang` tab of the Raindex app, we can see what is the result of composition for each of the scenarios present in the front matter. 
<img src="/img/rainlang_tab.png" />z 
- Following is the resultant composition for `limit-order.buy.prod` scenario.
```
/* 0. calculate-io */ 
using-words-from 0x662dFd6d5B6DF94E07A60954901D3001c24F856a 0xD6B34F97d4A8Cb38D0544dB241CB3f335866f490
  
  current-price: uniswap-v3-quote-exact-input(
    0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913 0x4200000000000000000000000000000000000006 
    1
    0x33128a8fC17869897dcE68Ed026d694621f6FDfD [uniswap-v3-init-code]
    [uniswap-v3-fee-medium]
  ),
  _: block-number(),
  final-amount: call<2>(),
  final-ratio: 0.0004;

/* 1. handle-io */ 
:;

/* 2. get-output-amount-prod */ 
max-output: 100;
```
- The composition contains of three sources indexed `0`,`1` and `2` of which the `calculate-io` source indexed `0` is the source that is evaluated to get the resultant order output amount and ratio and the `handle-io` source indexed `1` is evaluated after that. For simplicity, within the raindex app the statements within each source are also indexed starting from 0 with increments of 1 made to subsequent statements. For the current version of the raindex application, only source indexed 0 is evaled while running the scenarios.
- Elements can be accessed as `source-index.statement-index`. Example the `current-price` element can be accessed as `0.0` within in app, similarly `final-amount` and `final-amount` can be accessed as `0.2` and `0.3` respectively. The `max-output` element is not available within the `calculate-io` source, but we see a `call` made to source indexed `2` and `max-output` is the statement indexed `0` within that source, so the `max-output` element can be accessed as `0.2.0`.
- We also have the `Debug` tab within the raindex app that allows easy access to evaluated values present on the stack for a particular scenario run.  
<img src="/img/debug_tab.png" />