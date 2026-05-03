## Quant Finance
Reference implementations of portfolio construction and performance methodology applied during my tenure as Portfolio Manager at a HANFA-regulated voluntary pension fund (2020–2023).

---

## Portfolio Construction

The notebook file consists of two main ideas: 1) Projection of Expected Returns(and Covariances) and 2) Constructing portfolios using ER and COV. The key concept of the file is based on stochastic modeling. Stochastic modeling is a form of financial model that is used to help make investment decisions.
This type of modeling forecasts the probability of various outcomes under different conditions, using random variables. Stochastic modeling is inherently random, and uncertain factors are built into the model.

**Monte Carlo simulation**

The Monte Carlo simulation is one example of a stochastic model; it can simulate how prices may perform based on the probability distributions. Base case assumptions for performing M-C simulation are the following: a) using some historical inputs (e.g., daily prices of last two years), b) normal (Gaussian) distribution.

The brownian motion will be the main driver for estimating the return. It is a stochastic process used for modeling random behavior over time. A geometric Brownian motion (exponential Brownian motion) is a continuous-time stochastic process in which the logarithm of the randomly varying quantity follows a Brownian motion with drift.

A stochastic process St is said to follow a GBM if it satisfies the following stochastic differential equation (SDE):

dS(t) = muS(t)dt + sigmaS(t)dW(t), where W(t) is a Wiener process or Brownian motion, mu ('drift') and sigma ('volatility') are constants.

Drift — is its constant directional movement.

Volatility — represents market volatility that is multiplied with a random input, which in this case is a standard normal variable.

In this notebook we try to intervene into assumption /a)/, and not to allow historic prices to be the inputs for the model i.e. drift and volatility will not be based on historical prices.

Knowing that we need to replace inputs for drift (expected return) and volatility. It will be done by using two different models. For calculating expected returns it will be used Black-Litterman model and for volatility, it will be done by GARCH(1,1) model. To reiterate; expected return as an input for M-C simulation will be done by Black Litterman and for expected volatility, it will be used GARCH(1,1)

**Black-Litterman**

The award-winning model on portfolio construction (Markowitz MPT) starts with the idea of expected returns and covariances - first calculation and then optimized for weights. The logic of B-L is the opposite, it tries to reverse engineer the process since expected returns (ERs) are impossible to know and what usually happens is that calculated weights are not the most optimal. So, two GSs practitioners decided to go another way around, assuming that we already know what are the weights. Since we use publicly traded instruments for calculation they assumed that the market cap of stocks/ETFs are the optimal weights. For example, if we want to pair AAPL and GOOG for optimization then optimal weights are their market caps. From this ground, they build implied expected returns, which we did in the notebook with SPY and TLT. However, markets are not usually correct (in equilibrium) and prices can deviate from fundamentals. That is why they decided to incorporate personal (subjective) views into the calculation. Now we have prior expected returns (implied by the market) and subjective views. Pairing them together we get posterior expected returns that consists of market views and manager views, corrected for the degree of uncertainty of the manager. So the output of the model is the expected return and their covariances for the constituents that can be used in M-C simulation and Markowitz optimizations (covariances).

**GARCH(1,1) model**

On Google, it can be found lots of definitions for the model so it will not be covered here. However, it must be said we like that model because it takes into account different clusters in which markets found themselves from time to time. Meaning; the difference between using the last two years of daily returns to compute volatility and GARCH is in the model property where the GARCH takes into account that maybe history wouldn't repeat itself in the future. If the markets behave in an ordinary fashion with predictive deviations from their mean, then past prices would be great to compensate for that. But, since the markets can behave in completely different environments from one year to another it needs to be accounted for that when volatility estimates are done. Now, by no means do we believe that GARCH(1,1) is the perfect predictor for future volatility, but the „cluster“ characteristic of the model represents an improvement on to past returns. The projection will be done with GARCH(1,1) and no with ARCH model or GARCH(X,Y) because on the backtesting GARCH(1,1) usually showed the most stable outcomes. There are many more complex GARCH variations that we did not test and we encourage to play with them.

**Conclusion**

Finally, when we have all the outputs, we can plug them into M-C simulation for X projected days. On the last projected day, we can sort values from high to low and run statistics. The statistical results can show us what is the median of the series or the first and third quantiles. We can use these properties for projected ranges. For example, if Q1 = 100 and Q3 = 150, we can expect that on the last projected day prices should trade above Q1 and below Q3 75% of the time (or in that range Q1-Q3 50% of the time). That gives us confidence in our decisions when to buy or to sell, or we can use those levels to test for what weights should we get after optimization. In the notebook, we used median prices for the target prices for the next 252 days and then run the optimizations for the Sharpe ratio and for minimum volatilities.


## FILE: Portfolio Performance

The notebook file presents a couple of different metrics: Annualized Returns and Volatility, Sharpe ratio, Drawdown/Max DD, Skewness, Kurtosis, and Vars.

First, it displays four portfolios: Max Sharpe Ratio (MSR), Global Minimum Variance (GBM), Equally Weighted (EW), and the Benchmark (traditional) portfolio.

Based on the logic from /FILE: Portfolio Construction/ we created MSR and GBM portfolios (weights), while the other two are self-explainable. The projection started on 10/17/2021 for the next 252 days.  

Finally, it is presented the Summary Statistics and there it can be seen the best comparison between the portfolios.

