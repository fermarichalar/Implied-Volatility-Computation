# Implied Volatility Computation

Numerical estimation of Black–Scholes implied volatility using **Bisection** and **Newton–Raphson** methods.

---

## Overview

This repository implements and compares two root-finding algorithms for computing implied volatility from observed market option prices:

- **Newton–Raphson** (fast, derivative-based)
- **Bisection** (robust, bracket-based)

Given a market price $C_{mkt}$, we solve:

$$BS(\sigma) - C_{mkt} = 0$$

where $BS(\sigma)$ is the Black–Scholes price as a function of volatility.  
The solution $\sigma^*$ is the **implied volatility**.

---

## Mathematical Background

Under the Black–Scholes framework, the European call price is:

$$C = S_0 e^{-qT} N(d_1) - K e^{-rT} N(d_2)$$

Implied volatility does not admit a closed-form solution, so we compute it numerically.

---

## Newton–Raphson Method

We compute implied volatility by solving:

$$f(\sigma) = BS(\sigma) - C_{mkt} = 0$$

Newton–Raphson uses the slope of the pricing function to jump toward the root:

$$\sigma_{n+1} = \sigma_n - \frac{BS(\sigma_n) - C_{mkt}}{\mathrm{Vega}(\sigma_n)}$$

In the Black–Scholes model:

$$\mathrm{Vega}(\sigma) = S_0 e^{-qT}\,\phi(d_1)\sqrt{T}$$

**What it does:** Adjusts volatility using the pricing error scaled by sensitivity (vega).  
**Key property:** Quadratic convergence when vega is sufficiently large.  
**Limitation:** Can become unstable when vega is small (deep ITM/OTM options).

---

## Bisection Method

We search for $\sigma$ inside a bracket $[\sigma_L, \sigma_H]$ such that:

$$f(\sigma_L)\,f(\sigma_H) < 0$$

At each iteration, we update the midpoint:

$$\sigma_{\mathrm{mid}} = \frac{\sigma_L + \sigma_H}{2}$$

**What it does:** Shrinks an interval guaranteed to contain the root.  
**Key property:** Convergence is guaranteed if a valid bracket exists.  
**Limitation:** Linear convergence → slower than Newton–Raphson.

---

## Method Comparison

| Method | Convergence | Speed | Robustness |
|--------|------------|-------|------------|
| Bisection | Linear | Slower | Converges if bracket exists |
| Newton–Raphson | Quadratic | Very fast | May fail if vega is small |

---

## Practical Insights

- Newton–Raphson performs best for ATM options (high vega).
- Deep ITM/OTM options can produce small vega → instability.
- Safeguards (vega threshold, iteration cap) improve stability.

---

## Implementation Features

- Arbitrage lower bound checks
- Explicit volatility bracketing
- Adaptive bracket expansion
- Vega threshold safeguard
- Modular Black–Scholes pricing and vega functions

---

## References

- Capinski, M. J., & Zastawniak, T. (2012). *Numerical Methods in Finance with C++*. Cambridge University Press.
- Hull, J. C. (2018). *Options, Futures, and Other Derivatives* (10th ed.). Pearson.
- Manaster, S., & Koehler, G. (1982). The Calculation of Implied Variances from the Black–Scholes Model: A Note. *Journal of Finance*.
- Orlando, G., Mininni, R. M., & Bufalo, M. (2017). A review on implied volatility calculation. *Journal of Computational and Applied Mathematics*.
