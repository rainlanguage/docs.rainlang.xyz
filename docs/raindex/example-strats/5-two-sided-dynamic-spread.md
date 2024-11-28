---
sidebar_position: 5
---
# Two-sided dynamic spread strategies

## Introduction
The dynamic spread strategy for market-making uses time-based adjustments to maintain liquidity by narrowing spreads as market conditions stabilize, while recalculating averages and trade sizes to mitigate risks during trends.

## Summary
The dynamic spread strategy is designed for market making, where the goal is to provide continuous two-sided liquidity (both buys and sells) while managing risk and potentially generating profit. The strategy sets initial buy and sell points at configurable percentage spreads (e.g., 1%) above and below the current market price, with exponentially decaying spreads over time using a half-life curve - meaning the spread narrows progressively until a trade occurs.

The core mechanism involves tracking trade history and calculating weighted averages based on trade sizes, with larger trades occurring when price movements are faster or more significant. When a trade happens, the strategy resets its points: the entry side is set relative to the last trade price, while the exit side is set relative to the weighted average of previous trades. This creates an asymmetric spread that helps ensure profitability while adapting to market movements. The strategy can be configured to limit trade history retention, trading off guaranteed profitability for tighter spreads and more active market making.

This strategy is particularly suitable for markets that alternate between ranging and trending conditions, as it automatically adjusts its behavior based on market dynamics. In ranging markets, it provides regular liquidity with smaller trade sizes and tighter spreads as time passes. During trends, it protects itself by making larger trades early in the curve and using weighted averages to gradually adjust its middle price, preventing over-commitment to one side. The optional fast exit modification makes it more defensive during extended trends by aggressively resetting positions when counter-trend opportunities arise.

## Watch video
<iframe width="560" height="315" src="https://www.youtube.com/embed/ChKAr9uGrUY?si=5e1RaMDLjeq9iI34" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Link: [[https://www.youtube.com/embed/ChKAr9uGrUY?si=5e1RaMDLjeq9iI34](https://www.youtube.com/embed/ChKAr9uGrUY?si=5e1RaMDLjeq9iI34)]

<iframe width="560" height="315" src="https://www.youtube.com/embed/2KRAJreUA64?si=0hm5LmtWrvIHjrhz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Link: [https://www.youtube.com/embed/ChKAr9uGrUY?si=5e1RaMDLjeq9iI34](https://www.youtube.com/embed/2KRAJreUA64?si=0hm5LmtWrvIHjrhz)

## Getting started on Raindex webapp
Deploy an auction-based cost averaging strategy on Raindex, https://raindex.finance/. Here is an example:

![Strategy parameters](https://hackmd.io/_uploads/BktcDzN7ke.png)

## Example rainlang

The rainlang below is an example of a pre-configured [strategy]([https://raindex.finance/dynamic-spread/polygon-quick-wmatic?currentState=H4sIAAAAAAAAE81YWW%2FjOBL%2BK4Sf0oCdISlRR2MxQHxhgu3p7p2k0Q%2BLPFBkyeZEh1eU4nga%2Be9blOzYUpx0z%2B48TJCLZB1ffVWsYvJtZOtK1rDafZQ5jN6PbrflxBoNmuhdIXOjiN1UIDXZyxmwo%2FGz0hysqsymNmXhdNfwitaOpGVFclndQz3J5b0pVqSxYEltcpgk0qI%2FqX9vbJ1DUeN2icKmqPGLZOY%2FjdGm3pFkRwpZVeXWqXcOLJF2b5eoskAxhGLRrUxMZv6AMdmuTQakAiUz1WSydrryASq5Qvey0AQRaiAWhTu%2FaGKFmEll7L0luqmcRl1Boe0lhq6aCn%2Bvb2rYYMgVPBjY4raGTVbuHPpPezq%2Bneyh5KbMdquymGAw6n6yzRGJQr2io%2F1fX65n%2F%2FzHz18%2Ff%2FpALr7%2BenV7PXtHyoJ87rQuWw%2BnXP9W1g5kq9eG8ZZqaiDTdvT%2B399GiUGSihVaMAVGKrOJKY8wrrs9sqmMAnLRWd9A1Vr%2FaW%2F8BRaX96psVuu9HqbclG3GOwPI6ok%2BuYDL1SWhl%2Bydw5YbNEGfxqfQCnisJ21eJnmT1WaTGaiOKD%2Fi8T5tveMhKmMJftaILi9t3f7yXI9bk2W4%2FQDtdocbi01iIRQrLJjW%2FCX5IKsVhl80eQKV7bSU3NRNBSTrzjrd35t8Y0nSHIBlYC0p0xqKMbG5zLKhlT3%2BEg21Yq1uslfUkEJhDaKTK7wDCL511vOFaNvQ2uJ%2FZpKNR3gxLNRduveMsUvKHlHkQWYNrt3SMX485f1T3j8V%2FVPxdNfPV3uLsUwmsCnV%2BpioW9xvy2ctswd3jS4suFtqz9eQzMumqJGOti2QC4zwII%2FM1FtAlvaWbCv1nLn2Hp9aSKFyLay7vCDVmshGOVdjbAWuC7jEl07HVMT1n8wUYJ9ZDCg9zyNZl01FLjwUeHfkxC1PGOOtlCUXIe%2BJueWJmH8QY77fk2vXJ4LRQZBHUU%2BwXQ%2BTkcvHScfDMRG%2FykeTN%2FmBH5eSPR%2FYMvBqns9H3tdCxtsmU69l3dVwAs9Un96dvenj7e5zuQcv2hAPIdPeqncmzsRoipcxYkP7H2Lsa%2F3FMfJ%2BHIOA757u2sFRWrNXq8t7KIbD4qwLOkYLY6TG%2FaTjEL%2FRlqShiedJ80M27hzTB6KtG2InYwLr39F9tjl3PQaPB73g%2FchVvXN%2FLEuU7baOWcRLQ0dP59i4LtLS4djJPNu%2FUYbsSK0xJtQZ0cepmNHAX8ZCzCMIl16ULqgf0JkQgQx5HIV%2B7MOChW0xKIOdGfVYhE%2BaXZ6U2WEUn0xm5%2BRmKzcO3gGt6Mr1ewCfuT9FSDVqT5mOFhEsPbbgbBbHms2T%2BCrwfV97V%2FOU8ZC%2BjtBV6BHg10puNlid%2B3lPFqq0O1tDTm7bUhjAxpLDSVOXxWd8AbUF0S2xRbiqxARifq%2BLTVN%2FkAk4d%2BgK8NWFOS6aLBuPjP0KCfocva%2BrBsZHFrqsnfAgq8SgTD5prB7SINMo0jQQEIYqUjPOvdjj4Uws5ol35XucBxGIyGM9GoITFr7czGdHFnBFZqVx4Z6HsE3qIQSecsmF703D4MpnQSD8eBlKOeUQiimkVEI6E1Oa9iD0EjG9nb1MhNt8FQXU6wGKiMslFqUfziNJQ%2FASHYvpHKskSD3BfSa4l05lwt6oh8XtLy9hLHC4VUMgbtRN6tKuzTAdiiWa%2B1EgZeolQtFUeamOgkiIKKGIUXgs4Yn%2FOorbTze%2FXJ%2FM%2FtbJOfdniiHyPBHFqdIB6CQAGqW%2BCpXHtZ%2BGLBF%2BoiXlMfP%2Bz2Jo3Z9Jgc%2Fpj3wEf10KNtLoAYhAiCvBIIiol8qloHNfqkVEUykokylf8ngWiUAtXwfx%2Bep6fgThVuQjPpvK6n6IIcXnZJcIGKBIkWqRBmGgEi8MuJ%2FyiAdC4pNE%2BlgImKAo1F4SvJmJSzjCmFZGr5ALt08ubmp8x%2BIL%2FN15RNs0qwaA2Dyiyo%2BnyXSm2YzGjHk%2BXtVgKng8X8YLofgyjDz9Rm6WH357mZulc3gehT2DgkNARaIY9SH2pr4AJudxvIxBCKRLxFQwxafT11HYHgrk4d6BwL3zRLwsUnwU8eWVn0AQa8EDxv25L7yAxTwMllLNZMJDob35DxepWw2c%2F80G1%2BtTf1Jmw%2FuDsyIUntYhjUIlVcBkIhBX4tEo8LlSTHmeVoMe8t3Jb7vJ%2F%2Fd4f5xFYUrkqg9CU4hVlIqlBC9ivohVSsOFEjSYMc2jOFhEidDpG%2F3s%2BtPi4%2Bm%2FBWqoCmifx4sCKvz7%2BZXGcgB1psd7eItjJTAVi6VYIC4GDItXQUg1PgCopwVmSMR%2FusffPf0XTBO3w8YSAAA%3D](https://raindex.finance/auction-dca/bsc-busd-tft?currentState=H4sIAAAAAAAAE8VXbW%2FbNhD%2BK4Q%2BuYNtUH53viV1Awx7K5AM%2B7D2AyWdbSKSqJFUYi%2FIf98dKduirHYdUGCAG5S8Fz5399yReo2M1cLC7virKCC6iW7r1EpVskQYyFiqjGXiGbTYyXIXDc%2FaGzCplhWpotHDi6iYUQUwq56gZFulmSiV3YNudjTs6lzo%2FMjUM23KAoasNuiUCVZpMGBZUmc7sONP5eNeGoa%2FVOQ5gvgUhTA%2BRSyBVNQGmEFTkZEXVaMGI4c5MDxph0JCJUurWCHKIzMFudNu21xgILSsTsEwRMtkUYnUMrVlFopKaaGPaKyfEF2BFgWU1jiA6L3JBHtR%2Bsmw5IiOKsCtDIMUPosO2Hbrc2DYi7R7zAuDQ6VKdCVFzjKM5Igo3fGVlinmxe6FZS4nhoFI9wyPygDPxQKktdZo%2BmChwsRreJbwgtsZVLk6Er7fmqK8tvZQMzHpKKlNNrJbi%2Fqlr%2FYD5Dl7vH90Fbv7%2FWHDsPR3D%2B%2FHzuVVifs0HSo8ShlpTXTz52vkom2O9KcVEtd8GPlCkxYfxvRz%2F%2Fwf%2Fvnt8zDaSsgz7yaRJVUWHVGZRhXokShUXdoRHpbuL0HcOd4wVJAqYwPMpYFUlZl5dxUFFc7TjPhlKsxNwwSinKODd4Nh3WOgcBBFlWNJ5NYV6GKbCYl1xr3S2%2BJvtZhxfjqbDSYz9gNbcPfnnU%2BTy0PczsPrKYiPCALFtQU2QH1UfhZ5jZIFfxsGWntVazaYLnhbi5YdvQx5NXCYWnpu3VF8AXiiQ2crHh5MGx3dKSe%2FFN18PeGBfrPTNVjMG4tpPCeUAepm6%2B0zWV0K3tSZSt5f6wExEcVNtfoL7b1QM5M2jgGqd%2BY7Kmlz5huLLdD4VGtkrC8%2B%2FYfcv0jspATHgsqzS4%2B4szD8Vvl5GGohDiPX3g25L6H%2BIg6yqAvf%2FDjZ%2FgYXdn%2BsRaN8HTN1OLbEeTY2o%2BkriGT5JUSy%2FA%2BIGuXvgOjc%2Fx5WhxKnG%2Btb%2B1%2BE%2BtJPftf7CVhshtKX7axHQ5ntRf4MNNA9zJ4O%2FoBz5MgmvGljZHw8CdhOy1Z3eP1pWz9swHjVo%2F8t3d8gcbrodhnCWPbCOCnHvAOC96FYnNQncYjEra%2FU4zOW2TQE49bX0Gcn%2Ff751RkX9FbJZQmtMdHs%2BC6kQYHk62dDgh1LU%2Fear21bfyfbFn1cw%2FtOQMkAxrsx42POl4Hduy%2BzuoSDbRhd1LmVVS5BX9PaWKHx9dHW6Gm2s5g6TFQV3U3K4c0Fvp08Ttx5kumTf5KgiBD0dF885v13FAkOLWrgslU6XE5C6SSUzkPpPJCGjuNuiVvJOlX7q1mjQf%2B9ckZvU3cfnEhF470%2Ffz1J20gjEnrGDto8bnOej5et4HEVyFaBbBXI1oFsHcrmoXAeSjum627CsRvofTqS6pLcn5A8xJ1%2FbarGuNNBnn0n8m2lNufstfpnHfZPu30Q4gmhoQdu%2F7vwJnJDAs2unhFINO4kV7euk%2FiTrgW433sF3UQ0eFF6HkCUWh5Txnqb27dBKO6js6s7qrWKgKbRW99D%2B8dyqygZR1HkzTfc5eEtsgwZacjhYbXl93fxfD1d8Xi52Ewns%2FlskkzFcj2d3vP3k%2FuMp5Mk2bqKphK%2FltBuiZ98xyJROVXWVbvhAlXUfwEQrFOyYveYw4TU1qryo9iBe%2Ff7JT5oSAOTidH%2FWFa1%2FVkkQJ7RLWitMPCyznOM2%2FwBCXZkdGN1DcNLoD7uTqj0WdOJFdawfP%2FhdrqBLL2dr1ez5YrfiW06X6%2BTzWJ9u8n4armZL4JY41UrWCJh6z4hSj667xpqk%2F8x15hcDX%2FVEo9yeG6rSivs4%2Bhmi07g7R%2BvEQDv0Q8AAA%3D%3D)) on the Raindex webapp. 

The code implements the mathematical logic behind the dynamic spread strategy's core mechanics, particularly the time-based spread adjustments and trade sizing. For example, when the transcript mentions "exponentially doing this" with "halflife curves," this is directly implemented in the halflife function (section 8), which calculates how much the spread should narrow over time using a sophisticated power(0.5) calculation to maintain precision.

The "larger trades happening earlier in the curve" concept from the transcript is handled in sections 3 and 4, where amount-for-epoch and io-for-epoch calculate permissible trade sizes. For instance, if the market suddenly moves 5% (mul(last-io 1.05)), the strategy allows for larger trades compared to when the market is stable. This matches the transcript's explanation of making bigger trades when the market is trending to avoid getting stuck.

The trade history tracking discussed in the transcript is implemented through the storage mechanics in sections 6 and 7 (get-initial-time and get-last-trade), keeping track of timing and trade sizes. For example, if a trade occurs at $100, then moves to $105, the code tracks this through last-trade-time and last-trade-io variables, using these to calculate the next appropriate trade sizes and spreads based on the duration since the last trade (duration: sub(now() any(last-time initial-time))). This enables the strategy to maintain its adaptive spread behavior while protecting against adverse market movements.

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
