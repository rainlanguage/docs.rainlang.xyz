---
sidebar_position: 5
---
# Two-sided dynamic spread strategies

## Introduction
The dynamic spread strategy for market-making uses time-based adjustments to maintain liquidity by narrowing spreads as market conditions stabilize, while recalculating averages and trade sizes to mitigate risks during trends.

## Watch video
<iframe width="560" height="315" src="https://www.youtube.com/embed/ChKAr9uGrUY?si=5e1RaMDLjeq9iI34" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/2KRAJreUA64?si=0hm5LmtWrvIHjrhz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Getting started on Raindex webapp
Deploy an auction-based cost averaging strategy on Raindex, https://raindex.finance/. Here is an example:

QUICK<>WPOL (WMATIC) on Polygon.

![Screenshot 2024-10-25 at 13.43.21](https://hackmd.io/_uploads/r19RSFOgkl.png)

* Deployment	Token A<>Token B  on Polygon.
* Initial price (Token A per Token B)
* Next trade multiplier
* Time per halving (seconds)
* Maximum amount per auction (Token B)
* Minimum amount per auction (Token B)
* Deposits

## Example rainlang

The rainlang below is an example of a pre-configured [strategy](https://raindex.finance/dynamic-spread/polygon-quick-wmatic?currentState=H4sIAAAAAAAAE81YWW%2FjOBL%2BK4Sf0oCdISlRR2MxQHxhgu3p7p2k0Q%2BLPFBkyeZEh1eU4nga%2Be9blOzYUpx0z%2B48TJCLZB1ffVWsYvJtZOtK1rDafZQ5jN6PbrflxBoNmuhdIXOjiN1UIDXZyxmwo%2FGz0hysqsymNmXhdNfwitaOpGVFclndQz3J5b0pVqSxYEltcpgk0qI%2FqX9vbJ1DUeN2icKmqPGLZOY%2FjdGm3pFkRwpZVeXWqXcOLJF2b5eoskAxhGLRrUxMZv6AMdmuTQakAiUz1WSydrryASq5Qvey0AQRaiAWhTu%2FaGKFmEll7L0luqmcRl1Boe0lhq6aCn%2Bvb2rYYMgVPBjY4raGTVbuHPpPezq%2Bneyh5KbMdquymGAw6n6yzRGJQr2io%2F1fX65n%2F%2FzHz18%2Ff%2FpALr7%2BenV7PXtHyoJ87rQuWw%2BnXP9W1g5kq9eG8ZZqaiDTdvT%2B399GiUGSihVaMAVGKrOJKY8wrrs9sqmMAnLRWd9A1Vr%2FaW%2F8BRaX96psVuu9HqbclG3GOwPI6ok%2BuYDL1SWhl%2Bydw5YbNEGfxqfQCnisJ21eJnmT1WaTGaiOKD%2Fi8T5tveMhKmMJftaILi9t3f7yXI9bk2W4%2FQDtdocbi01iIRQrLJjW%2FCX5IKsVhl80eQKV7bSU3NRNBSTrzjrd35t8Y0nSHIBlYC0p0xqKMbG5zLKhlT3%2BEg21Yq1uslfUkEJhDaKTK7wDCL511vOFaNvQ2uJ%2FZpKNR3gxLNRduveMsUvKHlHkQWYNrt3SMX485f1T3j8V%2FVPxdNfPV3uLsUwmsCnV%2BpioW9xvy2ctswd3jS4suFtqz9eQzMumqJGOti2QC4zwII%2FM1FtAlvaWbCv1nLn2Hp9aSKFyLay7vCDVmshGOVdjbAWuC7jEl07HVMT1n8wUYJ9ZDCg9zyNZl01FLjwUeHfkxC1PGOOtlCUXIe%2BJueWJmH8QY77fk2vXJ4LRQZBHUU%2BwXQ%2BTkcvHScfDMRG%2FykeTN%2FmBH5eSPR%2FYMvBqns9H3tdCxtsmU69l3dVwAs9Un96dvenj7e5zuQcv2hAPIdPeqncmzsRoipcxYkP7H2Lsa%2F3FMfJ%2BHIOA757u2sFRWrNXq8t7KIbD4qwLOkYLY6TG%2FaTjEL%2FRlqShiedJ80M27hzTB6KtG2InYwLr39F9tjl3PQaPB73g%2FchVvXN%2FLEuU7baOWcRLQ0dP59i4LtLS4djJPNu%2FUYbsSK0xJtQZ0cepmNHAX8ZCzCMIl16ULqgf0JkQgQx5HIV%2B7MOChW0xKIOdGfVYhE%2BaXZ6U2WEUn0xm5%2BRmKzcO3gGt6Mr1ewCfuT9FSDVqT5mOFhEsPbbgbBbHms2T%2BCrwfV97V%2FOU8ZC%2BjtBV6BHg10puNlid%2B3lPFqq0O1tDTm7bUhjAxpLDSVOXxWd8AbUF0S2xRbiqxARifq%2BLTVN%2FkAk4d%2BgK8NWFOS6aLBuPjP0KCfocva%2BrBsZHFrqsnfAgq8SgTD5prB7SINMo0jQQEIYqUjPOvdjj4Uws5ol35XucBxGIyGM9GoITFr7czGdHFnBFZqVx4Z6HsE3qIQSecsmF703D4MpnQSD8eBlKOeUQiimkVEI6E1Oa9iD0EjG9nb1MhNt8FQXU6wGKiMslFqUfziNJQ%2FASHYvpHKskSD3BfSa4l05lwt6oh8XtLy9hLHC4VUMgbtRN6tKuzTAdiiWa%2B1EgZeolQtFUeamOgkiIKKGIUXgs4Yn%2FOorbTze%2FXJ%2FM%2FtbJOfdniiHyPBHFqdIB6CQAGqW%2BCpXHtZ%2BGLBF%2BoiXlMfP%2Bz2Jo3Z9Jgc%2Fpj3wEf10KNtLoAYhAiCvBIIiol8qloHNfqkVEUykokylf8ngWiUAtXwfx%2Bep6fgThVuQjPpvK6n6IIcXnZJcIGKBIkWqRBmGgEi8MuJ%2FyiAdC4pNE%2BlgImKAo1F4SvJmJSzjCmFZGr5ALt08ubmp8x%2BIL%2FN15RNs0qwaA2Dyiyo%2BnyXSm2YzGjHk%2BXtVgKng8X8YLofgyjDz9Rm6WH357mZulc3gehT2DgkNARaIY9SH2pr4AJudxvIxBCKRLxFQwxafT11HYHgrk4d6BwL3zRLwsUnwU8eWVn0AQa8EDxv25L7yAxTwMllLNZMJDob35DxepWw2c%2F80G1%2BtTf1Jmw%2FuDsyIUntYhjUIlVcBkIhBX4tEo8LlSTHmeVoMe8t3Jb7vJ%2F%2Fd4f5xFYUrkqg9CU4hVlIqlBC9ivohVSsOFEjSYMc2jOFhEidDpG%2F3s%2BtPi4%2Bm%2FBWqoCmifx4sCKvz7%2BZXGcgB1psd7eItjJTAVi6VYIC4GDItXQUg1PgCopwVmSMR%2FusffPf0XTBO3w8YSAAA%3D) on the Raindex webapp. 
```
/* 0. calculate-io */ 
using-words-from 0xF9323B7d23c655122Fb0272D989b83E105cBcf9d
epoch:call<2>(),
io: call<3>(epoch),
max-output: call<4>(epoch io),
_: io,
:call<5>(io);

/* 1. handle-io */ 
min-trade-amount: mul(20 0.9),
:ensure(greater-than-or-equal-to(if(call<6>() output-vault-decrease() input-vault-increase()) min-trade-amount) "Min trade amount."),
:call<7>();

/* 2. get-epoch */ 
last-time _: call<8>(),
duration: sub(now() last-time),
epochs: div(duration 7200);

/* 3. io-for-epoch */ 
epoch:,
last-io: call<8>(),
cost-basis-io: call<9>(),
max-next-trade: mul(max(cost-basis-io last-io) 1.01),
baseline: any(cost-basis-io last-io),
variable-component: sub(max-next-trade baseline),
decay: call<10>(epoch),
above-baseline: mul(variable-component decay),
_: add(baseline above-baseline);

/* 4. amount-for-epoch */ 
epoch io:,
decay: call<10>(epoch),
variable-component: sub(100 20),
base-amount: add(20 mul(variable-component decay)),
_: if(call<6>() base-amount mul(base-amount inv(io)));

/* 5. set-last-trade */ 
last-io:,
:set(hash(order-hash() "last-trade-time") now()),
:set(hash(order-hash() "last-trade-io") last-io),
:set(hash(order-hash() "last-trade-output-token") output-token());

/* 6. amount-is-output */ 
_: equal-to(0x0d500B1d8E8eF31E21C99d1Db9A6444d3ADf1270 output-token());

/* 7. set-cost-basis-io-ratio */ 
/* first reduce outstanding inventory */
  other-total-out-key: hash(order-hash() output-token() input-token()),
  other-vwaio-key: hash(order-hash() output-token() input-token() "cost-basis-io-ratio"),
  other-total-out: get(other-total-out-key),
  other-vwaio: get(other-vwaio-key),
  other-reduction-out: min(other-total-out input-vault-increase()),
  reduced-other-total-out: sub(other-total-out other-reduction-out),
  :set(other-total-out-key reduced-other-total-out),
  :set(other-vwaio-key every(reduced-other-total-out other-vwaio)),

  /* then increase our inventory */
  total-out-key: hash(order-hash() input-token() output-token()),
  this-vwaio-key: hash(order-hash() input-token() output-token() "cost-basis-io-ratio"),
  total-out: get(total-out-key),
  vwaio: get(this-vwaio-key),
  total-in: mul(total-out vwaio),
  remaining-in: sub(input-vault-increase() other-reduction-out),
  new-in: add(total-in remaining-in),
  remaining-out: div(remaining-in calculated-io-ratio()),
  new-out: add(total-out remaining-out),
  new-vwaio: every(new-out div(new-in any(new-out max-value()))),
  cap-out: if(call<6>() 1e50 div(1e50 any(new-vwaio calculated-io-ratio()))),
  capped-out: min(new-out cap-out),
  :set(total-out-key capped-out),
  :set(this-vwaio-key new-vwaio);

/* 8. get-last-trade */ 
stored-last-io:get(hash(order-hash() "last-trade-io")),
stored-last-output-token:get(hash(order-hash() "last-trade-output-token")),
last-time:get(hash(order-hash() "last-trade-time")),
_: if(equal-to(stored-last-output-token output-token()) stored-last-io inv(stored-last-io));

/* 9. get-cost-basis-io-ratio */ 
this-vwaio: get(hash(order-hash() input-token() output-token() "cost-basis-io-ratio")),
  other-vwaio: get(hash(order-hash() output-token() input-token() "cost-basis-io-ratio")),
  _: any(this-vwaio inv(any(other-vwaio max-value())));

/* 10. halflife */ 
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
```
