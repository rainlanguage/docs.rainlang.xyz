---
sidebar_position: 5
---
# Getting started
The easiest place to get started with rainlang strategies is on Raindex. Browse strategies on https://raindex.finance/ for configurable and easy to deploy strategies. 

Each strategy has a step-by-step configuration wizard with custom options as well as video explainers for complex strategies. 

When you've created the strategy parameters, you can see the rainlang before you deploy.

# Basic example, fixed limit order

Fixed Limit Orders enable trading assets at predetermined prices, providing precise control over entry and exit points across various markets. 

```
/* 0. calculate-io */ 
using-words-from 0xFe2411CDa193D9E4e83A5c234C7Fd320101883aC
max-output: max-value(),
io: if(
  equal-to(
    output-token()
    0x1D80c49BbBCd1C0911346656B529DF9E5c2F783d
  )
  100
  inv(100)
);

/* 1. handle-io */ 
:;
```

This example demonstrates a fixed limit order configuration on the Flare network for buying Wrapped Flare (WFLR) using Enosys USDT (eUSDT). The user sets the desired price at 100 eUSDT per WFLR and specifies a deposit amount of 100 eUSDT to fund the order. The Rainlang code implementation consists of two main components.

The first is a price calculation block that references the specific token address for the trade and establishes the maximum output values, while managing the input/output ratio calculations at the specified 100:1 rate. The second component is an execution block that handles the actual token swap mechanism.


# Moderate example, Auction-Based Cost Averaging (DCA)

Auction-Based Cost Averaging (DCA) strategy allows users to participate in periodic auctions to achieve optimal average prices while minimizing market impact and volatility exposure.

```
/* 0. calculate-io */ 
using-words-from 0x662dFd6d5B6DF94E07A60954901D3001c24F856a 0x662dFd6d5B6DF94E07A60954901D3001c24F856a
amount-epochs
trade-epochs:call<2>(),
max-output: call<3>(amount-epochs trade-epochs),
io: call<4>(trade-epochs),
:call<5>(io);

/* 1. handle-io */ 
min-amount: mul(10 0.9),
:ensure(greater-than-or-equal-to(output-vault-decrease() min-amount) "Min trade amount."),
used: get(hash(order-hash() "amount-used")),
:set(hash(order-hash() "amount-used") add(used output-vault-decrease()));

/* 2. get-epoch */ 
initial-time: call<6>(),
last-time _: call<7>(),
duration: sub(now() any(last-time initial-time)),
total-duration: sub(now() initial-time),
ratio-freeze-epochs: div(10 1000),
amount-epochs: div(total-duration 86400),
trade-epochs: saturating-sub(div(duration 3600) ratio-freeze-epochs);

/* 3. amount-for-epoch */ 
amount-epochs
trade-epochs:,
total-available: linear-growth(0 1000 amount-epochs),
used: get(hash(order-hash() "amount-used")),
unused: sub(total-available used),
decay: call<8>(trade-epochs),
shy-decay: every(greater-than(trade-epochs 0.05) decay),
variable-component: sub(100 10),
target-amount: add(10 mul(variable-component shy-decay)),
capped-unused: min(unused target-amount);

/* 4. io-for-epoch */ 
epoch:,
last-io: call<7>(),
max-next-trade: any(mul(last-io 1.05) call<9>()),
baseline-next-trade: mul(last-io 0.8),
real-baseline: max(baseline-next-trade call<10>()),
variable-component: saturating-sub(max-next-trade real-baseline),
above-baseline: mul(variable-component call<8>(epoch)),
_: add(real-baseline above-baseline);

/* 5. set-last-trade */ 
last-io:,
:set(hash(order-hash() "last-trade-time") now()),
:set(hash(order-hash() "last-trade-io") last-io);

/* 6. get-initial-time */ 
_:get(hash(order-hash() "initial-time"));

/* 7. get-last-trade */ 
last-time:get(hash(order-hash() "last-trade-time")),
last-io:get(hash(order-hash() "last-trade-io"));

/* 8. halflife */ 
epoch:,
/**
 * Shrinking the multiplier like this
 * then applying it 10 times allows for
 * better precision when max-io-ratio
 * is very large, e.g. ~1e10 or ~1e20+
 *
 * This works because `power` loses
 * precision on base `0.5` when the
 * exponent is large and can even go
 * to `0` while the io-ratio is still
 * large. Better to keep the multiplier
 * higher precision and drop the io-ratio
 * smoothly for as long as we can.
 */
multiplier:
  power(0.5 div(epoch 10)),
val:
  mul(
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
    multiplier
  );

/* 9. constant-initial-io */ 
_: 1;

/* 10. constant-baseline */ 
_: 0.01;
```

The Auction-Based Cost Averaging strategy implements a sophisticated approach to token sales through periodic auctions that optimize for price discovery while maintaining controlled market exposure. This example operates on a 24-hour budget period (86,400 seconds), with each period allocated 1,000 TFT for sale against BUSD on the BSC network. To manage market impact, trades are fragmented with size constraints between 10 and 100 TFT per execution.

The auction mechanism runs on hourly intervals (3,600 seconds) and employs dynamic pricing based on a baseline price of 0.01 BUSD per TFT. Each auction cycle begins at a 5% premium to the previous trade (1.05x multiplier) and can decrease to 80% of the previous trade price (0.8x multiplier), creating a price discovery range that adapts to market conditions. The system maintains state through persistent storage of trade history and uses a halflife decay function to smooth price transitions. This architecture ensures gradual price adaptation while preventing extreme price swings, effectively balancing the goals of market efficiency and controlled token distribution. The initial price anchor is set at 1 BUSD per TFT, with deposits capped at 1,000 TFT to maintain system liquidity constraints.

# Additional examples

Two-Sided Dynamic Spread strategy employs time-based adjustments to maintain liquidity by automatically narrowing spreads as market conditions stabilize. This strategy includes an optional fast exit feature for defensive trading during trends and recalculates trade sizes to mitigate risks. 

Grid Trading strategy places orders at predictable fixed price levels, featuring innovative automated grid point calculations and a unique recharge mechanism that allows streaming budgets to gradually refill levels from the baseline.

# Advanced examples

Advanced trading strategies using rainlang that you can try deploying, modifying, and simulating are on Github.

https://github.com/rainlanguage/raindex.pubstrats

The strat intentionally does no trading itself; it merely samples data and stores it under a predictable set of keys. Other strats can then be written that are read-only over these keys, keeping the overall system decoupled and easy to manage. As long as the sampler vault has enough WFLR in it to cover gas, there is an incentive for solvers to keep sampling indefinitely every time the strat comes off cooldown.
