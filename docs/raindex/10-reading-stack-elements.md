# Reading stack elements

## Overview
- Rainlang is a stack-based interpreted language, understanding stack element positions is crucial for debugging, charting, and backtesting. This guide explains how to read and reference stack elements within the Raindex application.
- Composed rainlang document for a scenario must be known in order to know the positions of the elements on the stack. Fully composed rainlang is available in the rainlang tab of the raindex editor.

<img src="/img/rainlang_tab.png" />

## Stack Overview
- Rainlang is a stack based language, where sources and statements are zero indexed and the elements can be accessed in source-statement hierarchy.
Eg : Reffering to our example, following is the composition for the `limit-order.buy.prod` scenario: 
```
/* 0. calculate-io */ 
using-words-from 0x662dFd6d5B6DF94E07A60954901D3001c24F856a 0xD6B34F97d4A8Cb38D0544dB241CB3f335866f490
  
  current-price: uniswap-v3-quotea-exact-input(
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
## Source Structure
- The source indexing starts from `0` with increments of `1` made to subsequent sources, the sources are commented for reference in the `Rainlang` tab. Eg: Source `calculate-io` is indexed `0`, `handle-io` is indexed `1` and so on.
- Source Indexed `0` and `1` are entrypoints for order execution within the OrderBook contract.

### Source Types
1. **Primary Source (`calculate-io`)**
   - Index: 0
   - Purpose: Main orderevaluation logic
   - Used for calculating order amount and ratio

2. **IO Handler Source (`handle-io`)**
   - Index: 1
   - Purpose: Post-calculation processing
   - Executed after primary `calculate-io` source indexed `0` is evaluated.

3. **Utility Sources**
   - Index: 2+
   - Indexed 2 and above are the utility sources which can be called by any of the above sources.

## Element Reference System

### Reference Pattern
- Elements can be referenced by the following pattern. 
```
<source-index>.<statement-index>[.<nested-index>]

```
where `source-index` is the index of the source, `statement-index` is the index of the statements within that source,
and the optional `nested-index` is the statement index within the call to the nested source.

### Components
- Example the `current-price` element can be accessed as `0.0` within in app, similarly `final-amount` and `final-ratio` can be accessed as `0.2` and `0.3` respectively. The `max-output` element is not available within the `calculate-io` source, but we see a `call` made to source indexed `2` and `max-output` is the statement indexed 0 within that source, so the `max-output` element can be accessed as `0.2.0`.
1. **Source Index**
   - Zero-based
   - Identifies source block
   - Example: `0` for `calculate-io`

2. **Statement Index**
   - Zero-based within source
   - Sequential per source
   - Example: `0.2` for third statement in first source

3. **Nested Index**
   - Optional
   - References nested elements
   - Example: `0.2.0` for first element in called source


## Element Access Examples

### Direct References
```plaintext
current-price  → 0.0
block-number   → 0.1
final-amount   → 0.2
final-ratio    → 0.3
```

### Nested References
```plaintext
max-output     → 0.2.0  (accessed via call<2>)
```

## Composition Analysis

### Source Structure
```rainlang
/* 0. calculate-io */
statement1  // index: 0.0
statement2  // index: 0.1
...

/* 1. handle-io */
statement1  // index: 1.0
...

/* 2. utility */
statement1  // index: 2.0
...
```

### Evaluation Order
- Within the OrderBook smart contract primary `calculate-io` source is evaluated first, with nested calls made to utility sources if any, followed by the evaluation of the `handle-io` source.
- Note that the current version of the raindex app does not evaluate the handle-io source, debug trace for which isn't available unlike the `calculate-io` source which has the complete debug stack trace.

## Debugging Tools

### Raindex Debug Tab
- Real-time stack inspection is available in the `Debug` section within the raindex editor for `calculate-io` source and calls made to the nested sources.

<img src="/img/debug_tab.png" />


