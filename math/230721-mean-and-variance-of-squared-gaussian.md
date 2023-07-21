Mean and Variance of Squared Gaussian
====

# Question

Given $$Y=X^2$$ where $$X \sim \phi(0, \sigma^2)$$, what is the mean and variance of the $$Y$$?

Since $$ \sigma^2 = E(X^2) - E(X)^2 $$, and $$E(X) = 0$$, $$E(X^2) = \sigma^2$$. Also, 

$$
VarX^2 = EX^4 - (EX^2)^2
$$

and the fourth moment $$EX^4$$ is equal to $$3\sigma^4$$ since

$$
\begin{aligned} 
\int \frac{x^4}{\sqrt{2\pi}\sigma}e^{-\frac{x^2}{2\sigma^2}}dx &= \frac{1}{\sqrt{2\pi}\sigma}\left ( -\sigma^2x^3e^{-{\frac{x^2}{2\sigma^2}}}|_{-\infty }^{+\infty } + 3\sigma^2\int x^2e^{-{\frac{x^2}{2\sigma^2}}}dx \right ) \\
&= \frac{1}{\sqrt{2\pi}\sigma} \left ( 0 - 3\sigma^4xe^{-{\frac{x^2}{2\sigma^2}}}|_{-\infty }^{+\infty } + 3\sigma^4\int e^{-{\frac{x^2}{2\sigma^2}}}dx \right ) \\
&= 3\sigma^4
\end{aligned} 
$$

(actually, $$E\left [  X^{2n}\right ] = (2n - 1)!!\sigma^{2n}$$, $$!!$$ is the [double factorial](https://en.wiktionary.org/wiki/double_factorial#:~:text=double%20factorial%20(plural%20double%20factorials,double%20exclamation%20mark%20(!!).))

thus 

$$
\begin{aligned}
VarX^2 &= EX^4 - (EX^2)^2 \\
&= 3\sigma^4 - \sigma^4 \\
&= 2\sigma^4
\end{aligned} 
$$

# Reference

1. [Mean and variance of Squared Gaussian](https://math.stackexchange.com/questions/620045/mean-and-variance-of-squared-gaussian-y-x2-where-x-sim-mathcaln0-sigma)
2. [Proving $$E(X^4) = 3\sigma^4$$](https://math.stackexchange.com/questions/1917647/proving-ex4-3%CF%834)