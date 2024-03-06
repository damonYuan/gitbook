# Quant Questions

## Q1
> Nine months call options with strikes 20 and 25 on a non-dividend-paying underlying asset with spot price $22 are trading for $5.5 and $1, respectively. Can you find an arbitrage?

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

The arbitrage strategy is to "buy low" $$\frac{1}{5}C_{1} + \frac{4}{5}C_{3}$$ and "sell high" $$C_{2}$$.  To normalize units, we multiply the positions by 500 to obtain the following arbitrage strategy: "buy low" $$100C_{1}+400C_{2}$$ and "sell high" $$500C_{2}$$. Note that buying $$100C_{1}$$ is equivalent to buying 100 units of the underlying asset since the asset does not pay dividends.

Arbitrage Strategy: 
- buy 100 units of the underlying asset for $2200;
- buy 400 calls with strike $$K_{3} = 25$$ for $400;
- sell 500 calls with strike $$K_{2} = 20$$ for 2750;
- realize a positive cash flow of $150.

The positive cash flow $150 represents risk-free profit since the arbitrage portfolio does not lose money at maturity: The value of the arbitrage portfolio at the maturity T of the options is 

$$
\begin{aligned}
V(T) &= 100S(T) - 500C_{2}(T) + 400C_{3}(T) \\
&= 100S(T) - 500\max(S(T) - 20, 0) + 400\max(S(T) -25, 0)
\end{aligned}
$$

If $$S(T) \leq 20$$,

$$
V(T) = 100S(T) \geq 0
$$

If $$20\lt S(T)\leq25$$,

$$
\begin{aligned}
V(T) &= 100S(T) - 500(S(T)-20) \\
&= 10000-400S(T) \geq 0
\end{aligned}
$$

If $$25 \lt S(T)$$,

$$
\begin{aligned}
V(T) &= 100S(T)-500(S(T)-20) \\
&\;+400(S(T)-25) \\
&= 0
\end{aligned}
$$

Note that $$150 = 500 \cdot (5.50 - 5.20)$$, i.e., the risk-free profit $150 is equal to the size of the convexity disparity $5.50 - $5.20 times the amplifier factor 500.

## Q2
> (i) What is the sum of the eigenvalues of the correlation matrix of n random variables?
> 
> (ii) Find a lower bound for the sum of the eigenvalues of the inverse of a nonsingular correlation matrix of n random variables.

Note that the eigenvalues of a matrix A are the roots of the characteristic polynomial $$P_{A}(t) = det(tI - A)$$ of the matrix A. And the calculation of determinant of a matrix could be find in [Determinant of a Matrix](https://www.mathsisfun.com/algebra/matrix-determinant.html).

$$
\begin{aligned}
det(tI -A) &= \prod_{i=1}^{n}(t- \lambda_{i}) \\
&= t^{n} - (\sum_{i=1}^{n}\lambda_{i})t^{n-1} + ... + (-1)^{n}\prod_{i=1}^{n}\lambda_{i}
\end{aligned}
$$
