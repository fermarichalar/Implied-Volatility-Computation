# Implied Volatility Computation

Numerical estimation of Black–Scholes implied volatility using 
Bisection and Newton–Raphson methods.

---

## Overview

This repository implements and compares two root-finding algorithms for 
computing implied volatility from observed market option prices:

- Newton–Raphson method (fast, derivative-based)
- Bisection method (robust, bracket-based)

Given a market price $C_{mkt}$, we solve:

$$
BS(\sigma) - C_{mkt} = 0
$$

where $BS(\sigma)$ is the Black–Scholes price as a function of volatility.

The solution $\sigma^*$ is the implied volatility.

---

## Mathematical Background

Under the Black–Scholes framework, the European call price is:

$$
C = S_0 e^{-qT} N(d_1) - K e^{-rT} N(d_2)
$$

Implied volatility does not admit a closed-form solution, so we compute it numerically.

---

### Newton–Raphson Method

We define:

$$
f(\sigma) = BS(\sigma) - C_{mkt}
$$

The update rule is:

$$
\sigma_{n+1} = \sigma_n - 
\frac{f(\sigma_n)}{f'(\sigma_n)}
$$

In the Black–Scholes model:

$$
f'(\sigma) = \text{Vega}(\sigma)
$$

with

$$
\text{Vega} = S_0 e^{-qT} \phi(d_1) \sqrt{T}
$$

Newton–Raphson has quadratic convergence when vega is sufficiently large.

---

### Bisection Method

We search for a root in an interval $[\sigma_L, \sigma_H]$ such that:

$$
f(\sigma_L) f(\sigma_H) < 0
$$

The midpoint is iteratively updated until convergence.

Bisection guarantees convergence provided a valid bracket exists, 
but converges linearly.

---

## Method Comparison

| Method | Convergence | Speed | Robustness |
|--------|------------|--------|------------|
| Bisection | Linear | Slower | Always converges if bracket exists |
| Newton–Raphson | Quadratic | Very fast | May fail if vega is small |

---

## Practical Insights

- Newton–Raphson performs best for ATM options (where vega is large).
- Deep ITM/OTM options may produce small vega → instability.
- Safeguards (vega threshold, iteration cap) improve stability.

---

## Implementation Features

- Arbitrage lower bound checks
- Explicit volatility bracketing
- Adaptive bracket expansion
- Vega threshold safeguard
- Clean modular Black–Scholes pricing and vega functions

---

## References


-Capinski, M. J., & Zastawniak, T. (2012). *Numerical Methods in Finance with C++*. Cambridge University Press.
- Hull, J. C. (2018). *Options, Futures, and Other Derivatives* (10th ed.). Pearson.
- Manaster, S., & Koehler, G. (1982). The Calculation of Implied Variances from the Black–Scholes Model: A Note. *Journal of Finance*. :contentReference[oaicite:0]{index=0}
- Orlando, G., Mininni, R. M., & Bufalo, M. (2017). A review on implied volatility calculation. *Journal of Computational and Applied Mathematics*. :contentReference[oaicite:1]{index=1}
