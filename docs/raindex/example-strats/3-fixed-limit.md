---
sidebar_position: 5
---
# Fiex Limit Strategy

## Introduction
# Fixed Limit Trading Strategy
The Fixed Limit Trading Strategy is a rules-based approach that uses predetermined price levels to enter and exit positions, and a systematic approach to managing risk.

## Getting started on Raindex webapp
Deploy a fixed limit strategy on Raindex, https://raindex.finance/. Here is an example:

Buy SFLR with WFLR on Flare.

<img width="1493" alt="Screenshot 2024-10-25 at 13 01 17" src="https://github.com/user-attachments/assets/79c13844-ebab-4df9-bc00-a145dc59064f">

- Select token pair
- Set price of Token A in Token B
- Set deposit amount
- Review, share, deploy

## Getting started on Raindex self-hosted app
```rainlang for fixed limit strtaegy
#calculate-io
using-words-from raindex-subparser
max-output: max-value(),
io: if(
  equal-to(
    output-token()
    fixed-io-output-token
  )
  fixed-io
  inv(fixed-io)
);

#handle-io
:;

#handle-add-order
:;
```

## Bindings
In the strategy bindings you can
- Choose your network
- Choose your token pair
- Set your price
- Set your quantity
- Set deployment

## Example

/* 0. calculate-io */ 
using-words-from 0xFe2411CDa193D9E4e83A5c234C7Fd320101883aC
max-output: max-value(),
io: if(
  equal-to(
    output-token()
    0x12e605bc104e93B45e1aD99F9e555f659051c2BB
  )
  5
  inv(5)
);

/* 1. handle-io */ 
:;
