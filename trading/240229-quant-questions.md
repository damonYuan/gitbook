# Quant Questions

> #### 1. Nine months call options with strikes 20 and 25 on a non-dividend-paying underlying asset with spot price $22 are trading for $5.5 and $1, respectively. Can you find an arbitrage?

Note that a call option with strike 0 on a non-dividend-paying underlying asset is the same as one unit of the asset, since the call with strike 0 will always be exercised at maturity by paying $0, i.e., the strike of the option, to receive one unit of the asset. Thus, we are implicitly given a third call option with strike K = 0 and price $22 (i.e., the spot price of the asset), and we can proceed to identify whether there is convexity arbitrage for these three call options.

Let $$K_{1}=0, K_{2}=20, K_{3}=25$$ and $$C_{1}=22, C_{2}=5.5, C_{3}=1$$. Note that $$20=\frac{1}{5}\cdot0 + \frac{4}{5}\cdot25$$, i.e.,

$$
K_{2} = \frac{1}{5}K_{1}+\frac{4}{5}K_{3}.
$$

Since

$$
\frac{1}{5}C_{1} + \frac{4}{5}C_{3} = 5.20 < 5.50 = C_{2},
$$

the convexity of option prices with respect to strike is violated.

![](https://raw.githubusercontent.com/Midtown-Innovation/quantech-weekly/main/resource/option_convexity.png "convexity of option prices")

The arbitrage strategy is to "buy low" $$\frac{1}{5}C_{1} + \frac{4}{5}C_{3}$$ and "sell high" $$C_{2}$$. 