---
sidebar_position: 5
---
# Auction based cost averaging

## Introduction
An auction-based cost averaging strategy helps reduce risk when investing. It involves placing incremental bids over time, similar to dollar-cost averaging, but in an auction format. The approach is designed to spread out investments to avoid market timing issues and capture potential price advantages as bids are won.

The setup typically involves setting bid amounts, timeframes, and price points based on market conditions and the asset’s volatility.

## Watch video
<iframe width="1531" height="749" src="https://www.youtube.com/embed/jyHKEt80MKs" title="Understanding Auction Based Cost Averaging Strategy 📊" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Getting started on Raindex webapp
Deploy an auction-based cost averaging strategy on Raindex, https://raindex.finance/. Here is an example:

Sell WBTC for WETH on Arbitrum.

![Screenshot 2024-10-25 at 13.19.12](https://hackmd.io/_uploads/HyRcxY_x1g.png)

- Select token pair
- Budget period (in seconds)
- Budget (Token A per period)
- Maximum trade size (Token A)
- Minimum trade size (Token A)
- Auction period (in seconds)
- Baseline Token B per Token A
- Auction start multiplier
- Auction end multiplier
- Kickoff Token A per Token A
- Deposits

## Example rainlang

The rainlang below is an example of a pre-configured [strategy](https://raindex.finance/auction-dca/arbitrum-wbtc-weth?currentState=H4sIAAAAAAAAE81YW2%2FbOhL%2BK4T3JVnYjiRbzuUtOW2xxe45W6AFisVpgUNJlM2NbhCpON4i%2F32%2FISWblJVuD9CHBdI0ImeGc%2F1myG8zpVuuxfbwGy%2FF7G5236Va1hVLuBIZS2ulGX8SLd%2FKajubH6nfCJW2siFSMH3c84apuhRM14%2BiYnndMl7VeifafqUV267gbXFg9RMtylLMWacglHHWtEIJzZIu2wq9%2FFJ92knF8JPyooASX2a%2BGl9mLBEp75RgCqw8Iyl1BwpGAgvBcNIWm6SVrHTNSl4dmCpJXGuW1UkNqJZ1qVAM2jJZNjzVrM6ZFmVTt7w9gLl9hHYlOEpRaWUUhPTeE2xft4%2BKJQcIagSWMhjJrReNYnlufaDYXuod%2FMLEc1NXECV5wTJYcoCW5vimlSn8ondcM%2BMTxQRPdwxHZQLnIgBp17Zg%2FahFA8e34kmKPZYz0RT1gfT7Zx%2BUb84aKHmbSN125WKf6HSxF3oHrsrG%2FKMoCvb54dMvJnCf3376G0MG3PccSyPeDfcH3mqZygbGkua%2Fk%2Bp5IZtGkMVfL3ZaN%2Bru6qoroLiquypbljD4cPWXE9UleYxC0ZCTXj%2FcGA1LaiW1mt39%2Fm1mnDm2CESlxGown9lsItpgHiyDIAjNf%2BY3%2FaJ%2F0deXr%2FNZLkWRWaGJrCiNIJZyYtGIdsFLaK4XODp1fPVgkpSBQNYZu4D5SqR1lanLMzdRlticpmRWsFv3aUf5bXLPioGRX6p3sF4887IpkAEyN%2Flw4s64RFphrbLc%2BLnZrINgOJ1dRGv2V7YJzK9L6zbjkdD1yLfBjA9QA9sdIngBehA%2F8aLDziZ4mXtUu7pr2cVqE7hU9Dmiy5DGF0Ynh858jwj3QjzSoeubwD%2BYFka0q4DkknXxbRR49P3KmGET9xyrMCYtPa37pZevxHUKeR9pCvp0tC9MfmK%2FD9h0rK0YAg9DDtyhmGe2hBM3b5Y%2FFm4O5iHalLw2%2FuYvc8Jeom4TQFFdZKfqMcfBB04OBL69JX9eGEjpc%2Fxk76%2F8WZZdaQEHaPofYW2fNrjsqScMJ0RBbRwRuQfE7%2Bgkq9d0ktWf0amn%2Fhk6HaHAKjbKjaFT%2FigUcJ9e2o5jYCARGlVR2dAd6agZsB0vngxgQs17yzIQQMJUM5mfWonNECOCCSDPwR5ncqpXo%2BFKCfVKQp6rHFo88ICIauqEQ0cIYu9zS0Icp56Ixon2AdU1VbfJWCositK8z3hhpo9CVsIceUbVn%2B4ZiTKIRoSM5xp%2F9ipfwA35HiA1OBun03FhcDlnYTzJGhlWdc4bObz0h6rZkEgTYPvWuB48FnEhL4w8YKJPB8gs%2Fcql97EyvJmg%2FxGg7jUZzLr21bieVGMgDoOREsGUFpuBPAp9Tcz3GXl41GW98pUx3%2Beqrwf66VYzQvYhjxxEHzLLi%2Fd0wSYAVuqQE5jiJYuZ19x6MRlp8Yq7A60ZMSsYgtH1Qi7Fkv0Rsiv2%2FMelGTmxC7lXJBKzpazHVXmW4Pu6A%2FKXAjBQUX0bjDvOU%2FuhTDyhpAdGVkUwwE03iQcNU3xTO0EPAl4G9CuTiieF8IrytTwPAyc3TpGLTsuRuxw7rfy0vDpRr87CWYln3YNxieFSNoUU7TkiG4RhHsVEozhuk52YQ2m%2Bqo2hBcd1w4YPK48yfbRTPLZIA6dxnIOmDXS4DI9Y5kjbc4N74zg6iObKt2lkjSGuyOdyZzyabKeQhzaenZLFp1NS%2BIz83cjfjf3d2Nv1BYffidWQON8NGk1KPytkJoPdNkLD0Z8IX7C8%2FVnhI7PAE96cBe%2B3WosBOnAm7pdUuW7nk9uqxu2UNJOabWt0gkQU9d6vRvaegERPAdZRGF07WpmZWy4JzqEp7rwK99sfLP6Ren3b2xkAAlsBYRZQwlfb4BsrDqOSi9suxgfLayep8OXt3Xh7N97erbd36%2B%2FF%2Fmbs745Yb8eJDF%2FScLWQ9Slp%2Fw5MIEj4302k5x53DAsrA6rkslVuXvrvC5RGcKcWJiM5vXOc8rG0Taaf6cw8lAikuzgXPKcxxQgwsyA4EFBRoBP0sU95kXYFPWGwvK1LktAKp32Nsh7S5ETX6m8rsTMknvIkCiDvX3VnjrRlSrnfYe7dya15L6IkMhln313IC8oUtQFC7bqG%2FCe4wl1pbkq8r2V2oAPMkXSENPAZB4O2vWBTzS29TkCrRWS7IAwm%2FasaBSKyHjekOR%2FaQ36q5yTw3x38uuPDRsKLooFYUxWZyCnqAug0bdOS3VcHVnVlQtdhSYFg4SIMng1NK3jRR%2FLkt9z23U%2B%2BN7ctPTg58v0Wff4iMt1lj%2BUQn5ZjZzkM3M5OXRklMlSIoqem6UeTu9kmgBpn12t0D9JufAvtl8cXQbs8eRe7m9F8i93jnHc3i%2Bh7elCw%2FdDfnupNFjRA55T%2B3SwOZi9TT1Hvq7wmHxx4WfSPqOOnKZ5lCIEiwc9RHvEoXq8erjf363Czide37645f4jEdfwg8oCL%2FJf4IcgNmqSy5AX4buYzdSiTuoCIHmp6JPrcog8iU2nxZfA1AVlEL1xJp3VdfeBbYRLBfuKSb4c1DTe8r5pO%2F4Ojr0AYxKJP1PBA1RUF7FefRQL5sztYI%2BYne635%2Fw8Wv6KFfd90tbiJ%2BLv17Xp9%2FeaGB9dilWS38cObIIs3%2BSqO1mEcrfIHnoSeFqGnBmDuXI23hJIzVMR%2FAauWbU1LFwAA) on the Raindex webapp. 
```
/* 0. calculate-io */ 
using-words-from 0xb06202aA3Fe7d85171fB7aA5f17011d17E63f382
amount-epochs
trade-epochs:call<2>(),
max-output: call<3>(amount-epochs),
io: call<4>(trade-epochs),
:call<5>(io);

/* 1. handle-io */ 
:ensure(greater-than-or-equal-to(output-vault-decrease() 1) "Min trade amount."),
used: get(hash(order-hash() "amount-used")),
:set(hash(order-hash() "amount-used") add(used output-vault-decrease()));

/* 2. get-epoch */ 
initial-time: call<6>(),
last-time _: call<7>(),
duration: sub(now() any(last-time initial-time)),
total-duration: sub(now() initial-time),
amount-epochs: div(total-duration 60),
trade-epochs: div(duration 3600);

/* 3. amount-for-epoch */ 
amount-epochs:,
total-available: linear-growth(1 1 amount-epochs),
used: get(hash(order-hash() "amount-used")),
unused: sub(total-available used),
capped-unused: min(unused 1);

/* 4. io-for-epoch */ 
epoch:,
last-io: call<7>(),
max-next-trade: any(mul(last-io 1.01) 50),
baseline-next-trade: mul(last-io 0.95),
real-baseline: max(baseline-next-trade 20),
variable-component: saturating-sub(max-next-trade real-baseline),
above-baseline: call<8>(variable-component epoch),
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
max-val epoch:,
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
    max-val
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

