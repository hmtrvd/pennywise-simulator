# PennyWise 💰
### A Monte Carlo Wealth Simulator for India's UPI-Native Working Class

---

## The Problem

India processes over 13 billion UPI transactions every month. A daily wage worker buys chai for ₹10, pays rent in instalments, sends money home — all through UPI. The digital payment revolution has reached everyone.

The stock market hasn't.

The prevailing belief among India's working and middle class is simple: *investing requires large capital*. With no lump sum to deploy, millions of UPI users remain locked out of wealth generation entirely — even as their phones already hold the infrastructure to change that.

**What if the spare change from those billions of daily transactions could be pooled and invested?**

---

## What PennyWise Does

PennyWise is a Python-based stochastic wealth simulator that models the viability of a micro-investment platform built on UPI round-ups.

The core idea: round up every UPI transaction to the nearest rupee. Pool that spare change — we're talking 50 paisa at a time — across thousands of users into a single investment fund. Track ownership fractionally. Invest in NIFTY 50.

To test whether this actually works mathematically, I built a Monte Carlo simulation using **Geometric Brownian Motion** on 15 years of real NIFTY 50 data.

---

## The Math

**Why Geometric Brownian Motion?**

GBM is the standard framework for modelling asset price dynamics because:
- It ensures prices always remain positive (unlike simple random walks)
- It models price as a random walk with drift — capturing both long-term market growth (μ) and short-term volatility (σ)
- It assumes log-normal distribution of returns, consistent with observed behaviour in equity markets
- It scales efficiently for Monte Carlo simulation across thousands of paths

**The simulation:**
```
dS = μS dt + σS dW
```
Where μ = annual drift (expected return), σ = annual volatility, dW = Wiener process (random market shock)

Parameters were derived from real NIFTY 50 data (15-year period) using:
- μ = mean daily log return × 252 trading days
- σ = std dev of daily log returns × √252

---

## Simulation Parameters

| Parameter | Value |
|-----------|-------|
| Users | 1,000 |
| Avg round-up per transaction | ₹0.50 |
| Transactions per user per day | 2 |
| Daily pool | ₹1,000 |
| Simulation horizon | 5 years |
| Number of simulated paths | 5,000 |
| PennyWise fee | 15% of profits only |

---

## Key Findings

- **50-paisa round-ups yield a 25%+ wealth multiplier** over 5 years at median market performance
- Right-skewed distribution of outcomes: the potential for outlier growth significantly outweighs downside risk
- **95% Value at Risk (VaR):** Even in bear market scenarios, the pooling strategy remains robust for the average user
- Probability of beating cash: calculated across all 5,000 simulated futures

---

## Business Model

PennyWise takes a **15% fee on profits only** — never on principal. Users always get their money back. The startup only earns when users earn.

This structure aligns incentives: PennyWise wins only if its users win.

---

## Tech Stack

- Python (Google Colab)
- `yfinance` — real NIFTY 50 market data
- `numpy` — vectorized GBM computation
- `pandas` — data processing
- `matplotlib` + `seaborn` — visualisation (fan charts, probability distributions)

---

## Outputs

1. **Fan Chart** — 90% confidence interval of portfolio growth across 5,000 futures vs. cash baseline
2. **Probability Distribution** — histogram of final portfolio values showing right-skewed outcome distribution
3. **Quant Metrics** — 95% VaR, probability of beating cash, per-user breakdown

---

## Limitations & Next Steps

- Model assumes constant volatility (real markets exhibit volatility clustering)
- Regulatory framework for pooled micro-investment in India requires further research
- Stress testing under 2008/2020-level bear market parameters in progress
- Fractional ownership tracking mechanism to be modelled

---

## Author

**Himani Trivedi** — Class XII, Delhi Public School Patna  
Interested in quantitative finance, capital markets, and the overlap between economics and computation.

[LinkedIn](https://www.linkedin.com/in/ACoAAFLL-n8BzE3NTbi5rv3CaNY8chcfwBc08lQ)
