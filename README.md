# PennyWise

A Monte Carlo simulation I wrote to test whether UPI round-up investing actually makes sense for someone who has no lump sum to invest. Python, runs in Colab.

## What it does

Rounds every UPI transaction up to the nearest rupee, pools the spare change across 1,000 users, puts the pool into the NIFTY 50, and simulates 5,000 five-year futures to see what happens to the balance.

## Method

- Data: 15 years of NIFTY 50 daily closes, pulled with `yfinance`.
- Drift and volatility estimated from daily log returns: mu = mean x 252, sigma = std x sqrt(252).
- Price paths generated with Geometric Brownian Motion, vectorised in NumPy.
- 5,000 paths, 5-year horizon. 2 transactions per user per day at an average round-up of 50 paise, which pools to roughly Rs 1,000 a day across 1,000 users.
- Everything is compared against a cash savings baseline. I also pull out the 95% VaR and the share of futures in which the pool beats cash.

## What came out

At median market performance the pool ends around 25% above the cash baseline after five years. The distribution is right-skewed, so most outcomes sit near the median and a small number run far above it. In the bad tail (95% VaR) the pooled strategy still holds up against cash for the average user, but that number leans heavily on the drift assumption and I would not trust it in a 2008-style drawdown.

Two charts come out of the notebook: a fan chart showing the 90% band across all paths, and a histogram of final portfolio values.

## What is wrong with it

- Constant volatility. Real markets have volatility clustering, so the tails here are too thin and the bad outcomes are understated.
- GBM assumes log-normal returns. NIFTY returns have fatter tails than that, which is the same problem from a different angle.
- The 15% fee on profits is a number I picked to see what happens, not something I tested or justified.
- No regulatory work at all. Pooled micro-investing in India would need a legal opinion long before anyone builds it, and I have not looked into that.
- Fractional ownership is not modelled. The simulation just assumes the pool can be split perfectly, which is the hard part in practice.

## Next

- Stress test with the 2008 and 2020 volatility windows instead of a 15-year average.
- Fit a GARCH model for volatility instead of using a constant sigma.
- Sensitivity table across round-up sizes, user counts and horizons.

## Running it

Open `pennywise-simulator.ipynb` in Colab and run top to bottom. Needs `yfinance`, `numpy`, `pandas`, `matplotlib` and `seaborn`; the data pull is the only step that needs a network connection.
