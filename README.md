# Quant Portfolio Optimizer

**Author** : Florent Yigbe  
**Program** : BSc, Mathematics in Business and Economics  
**Date** :  July 2026  

---

## Overview

A quantitative portfolio optimizer built in Python, applied to a diversified
universe of 29 global assets spanning US equities, international equities,
fixed income, and alternative assets. The optimizer uses mean-variance
optimization (SLSQP, via `scipy.optimize`) to identify the Maximum Sharpe
Ratio and Minimum Volatility portfolios, and benchmarks their risk-adjusted
performance against an Equal-Weight portfolio and the S&P 500.

The project is structured as follows:

| Step | Section | Description |
|------|---------|-------------|
| 1 | Data Collection | 10 years of adjusted daily price data (2015-2025), computed daily returns |
| 2 | Asset Risk Metrics | Annualized return, volatility, Sharpe ratio per asset, correlation heatmap |
| 3 | Equal-Weight Portfolio |  1/N baseline for comparison |
| 4 | Portfolio Optimization | SLSQP mean-variance optimization - Max Sharpe & Min Volatility portfolios |
| 5 | Value at Risk & CVaR | Historical VaR and CVaR at 95% and 99% confidence, in € terms |
| 6 | Cumulative Returns | Growth of €1 - Max Sharpe vs Equal-Weight vs S&P 500 |
| 7 | Summary | Consolidated performance table (return, volatility, Sharpe, VaR/CVaR, cumulative return) with top holdings |
| 8 | Limitations & Future Improvements | Critical review of methodology, data, and risk modeling assumptions, with a roadmap for extensions |

---

## Asset Universe

The 29-asset universe is built around four diversification axes:

- **US Equities (10)** - sector diversification across Technology, Healthcare,
  Financials, Consumer, Energy, Industrials, Utilities, and Real Estate
  (e.g. MSFT, NVDA, LLY, JPM, XOM).
- **International Equities (10)** - geographic diversification across Europe,
  Japan, Taiwan, China, India, Brazil, and Canada (e.g. ASML, SAP, TSM, BABA,
  HDB, VALE).
- **Fixed Income (5)** - US Treasuries across maturities, TIPS, international
  and emerging-market bonds (TLT, IEF, TIP, BNDX, EMB), used for
  de-correlation from equities.
- **Alternative Assets (4)** - commodities as inflation hedges and safe
  havens: gold, silver, oil, and agriculture (GLD, SLV, USO, DBA).

## Methodology

- **Optimization**: Mean-variance optimization solved with `scipy.optimize.minimize`
  (SLSQP method), subject to a fully-invested constraint (weights sum to 1)
  and long-only bounds (0 ≤ w ≤ 1).
- **Expected return**: Arithmetic mean of daily returns, annualized (×252
  trading days) - standard convention for Markowitz optimization.
- **Risk**: Annualized covariance matrix of daily returns.
- **Risk-free rate**: Fixed at 4.5%, used in the Sharpe ratio calculation.
- **VaR / CVaR**: Computed historically (empirical percentiles), not
  parametric or simulation-based.

---

## Limitations & Future Improvements

### Current Limitations

**Methodology**
- **In-sample optimization (look-ahead bias)**: weights are optimized once
  over the entire 2016-2026 window using the full history, including data
  that would not have been available at the time of investing. Live
  performance would likely be materially worse.
- **Static allocation**: no rebalancing over the period - weights are fixed
  at t=0 and never adjusted.
- **Mean-variance framework only**: assumes returns are normally
  distributed and ignores skewness, fat tails, and regime changes.
- **Arithmetic mean returns**: overstates expected return relative to what
  would actually be realized (CAGR), especially for volatile assets.
- **No transaction costs, slippage, or taxes**: the optimizer assumes
  frictionless trading.
- **No liquidity or capacity constraints**: assumes any position size is
  executable at the observed price.

**Data**
- **Survivorship bias**: only currently-listed, well-known large caps are
  used - no delisted or failed companies, which biases historical returns
  upward.
- **Currency risk not modeled explicitly**: international tickers are
  priced in USD via ADR/local listing; underlying FX exposure and hedging
  costs are not isolated.
- **Fixed universe of 29 tickers**: hand-picked for sector/geographic
  diversification, not derived from a systematic screening process.
- **Single fixed risk-free rate**: constant across 10 years, when the
  actual risk-free rate varied significantly (near-zero 2015–2021, higher
  post-2022).

**Risk Modeling**
- **Historical VaR/CVaR only**: based on empirical percentiles of past
  returns, not parametric or Monte Carlo methods, and doesn't account for
  tail risk beyond the observed sample.
- **Covariance matrix instability**: a single covariance matrix over 10
  years assumes stable correlations, while correlations are known to shift
  substantially in crises (e.g. 2020, 2022).

### Possible Improvements

**Methodology**
- Implement a **walk-forward / rolling window backtest**: re-optimize
  periodically using only data available up to that point, then evaluate
  out-of-sample performance.
- Add a **rebalancing schedule** with associated transaction costs.
- Extend beyond mean-variance to **robust optimization** (Black-Litterman,
  Hierarchical Risk Parity).
- Add **risk-parity** and **maximum diversification** portfolios as further
  benchmarks.
- Report **CAGR / geometric returns** for performance communication, while
  keeping arithmetic returns for the optimization step itself.

**Data**
- Systematize the **ticker universe** via factor screens (market cap,
  sector, liquidity) rather than manual selection.
- Model **currency exposure and FX-hedged vs unhedged returns** separately
  for international holdings.
- Use a **time-varying risk-free rate** instead of a constant.
- Address **track record length mismatches** across tickers with more
  careful alignment or an explicit minimum-history filter.

**Risk Modeling**
- Add **Monte Carlo simulation** for VaR/CVaR to capture tail scenarios not
  present in the historical sample.
- Use **shrinkage estimators** (e.g. Ledoit-Wolf) for the covariance
  matrix to reduce estimation noise.
- Add **stress testing** against specific historical crisis periods (2020
  COVID crash, 2022 rate-hike selloff).
- Incorporate **factor exposure analysis** (market, size, value, momentum)
  to understand portfolio behavior beyond aggregate statistics.
- Add **Maximum Drawdown** and **Calmar Ratio** as complementary
  downside-risk metrics.

