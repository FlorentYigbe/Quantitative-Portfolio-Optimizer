# Portfolio Optimization Project

This project uses quantitative finance models to analyze and optimize a multi-asset portfolio.

##  Features & Asset Classes
The model processes **30 highly diversified global assets** categorized into 4 distinct groups:
1. **US Equities**: Sector-diversified stocks (Tech, Healthcare, Financials, etc.).
2. **International Equities**: Geographic diversification across Europe, Asia, and Emerging Markets.
3. **Fixed Income**: US and international bonds acting as low-correlation defensive anchors.
4. **Alternative Assets**: Commodities (Gold, Oil) and Digital Assets for maximum diversification.

##  Methodology
* **Data Source**: Historical daily closing prices downloaded via `yfinance`.
* **Metrics Computed**: Annualized Mean Returns, Annualized Volatility, and Covariance Matrix.
* **Risk Management**: Sharpe Ratio calculation.
* **Visualization**: Interactive cumulative return line charts comparing the portfolio against individual assets.

##  Requirements & Installation
To run the code, install the required packages:
```bash
pip install numpy pandas matplotlib seaborn yfinance
```
