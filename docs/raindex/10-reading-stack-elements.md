# Reading stack elements

## Overview
Rainlang is a stack-based interpreted language where understanding stack element positions is crucial for debugging, charting, and backtesting. This guide explains how to read and reference stack elements within the Raindex application.

<img src="/img/rainlang_tab.png" />z 

## Stack Architecture

### Basic Concepts
- Stack-based execution model
- Zero-based indexing
- Source and statement hierarchy
- Element reference patterns

## Source Structure

### Source Types
1. **Primary Source (`calculate-io`)**
   - Index: 0
   - Purpose: Main evaluation logic
   - Used for order output calculations

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

2. **IO Handler Source (`handle-io`)**
   - Index: 1
   - Purpose: Post-calculation processing
   - Executed after primary evaluation

3. **Utility Sources**
   - Index: 2+
   - Purpose: Support functions
   - Called by primary source

## Element Reference System

### Reference Pattern
```
<source-index>.<statement-index>[.<nested-index>]
```

### Components
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
1. Primary source evaluation
2. Stack element population
3. Utility source calls
4. IO handler execution

## Debugging Tools

### Raindex Debug Tab
- Real-time stack inspection
- Element value visualization
- Statement execution tracking

<img src="/img/debug_tab.png" />

### Debug Features
1. **Stack Viewer**
   - Current stack state
   - Element values
   - Reference patterns

2. **Execution Trace**
   - Statement sequence
   - Call hierarchy
   - Stack mutations
