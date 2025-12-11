# Options Pricing & Greeks with Black–Scholes in Python

This project is a simple, self-contained Python script that:

- Fetches **options chain data** and **historical price data** for a given ticker (JPM by default) using `yfinance`
- Plots **historical stock prices**
- Implements the **Black–Scholes pricing model** for European call and put options
- Extends the model to compute **Greeks** (Delta, Gamma, Theta, Vega, Rho)
- Visualizes how **call Delta** changes with the underlying stock price

It’s intended as an educational / exploratory tool for learning options pricing and the Black–Scholes framework.

---

## Features

1. **Data Fetching (via Yahoo Finance)**
   - `fetch_options_data(ticker_symbol)`  
     - Fetches the **nearest expiration** options chain for the specified ticker.
     - Returns DataFrames for **calls** and **puts**.
   - `fetch_stock_data(ticker_symbol, period="1y")`  
     - Fetches **historical OHLCV data** for the given ticker and period (default: 1 year).

2. **Visualization**
   - Plots **historical closing prices** using `matplotlib`:
     - Example: JPM 1-year historical close price.

3. **Black–Scholes Pricing**
   - `BlackScholesModel`:
     - Inputs:
       - `S` – underlying price
       - `K` – strike price
       - `T` – time to expiration in **years**
       - `r` – risk-free interest rate (annualized, continuous compounding)
       - `sigma` – volatility of the underlying (annualized)
     - Methods:
       - `d1()`, `d2()` – standard Black–Scholes intermediary terms
       - `call_option_price()` – theoretical European call price
       - `put_option_price()` – theoretical European put price

   - Example in the script:
     ```python
     bsm = BlackScholesModel(S=100, K=100, T=1, r=0.05, sigma=0.2)
     print(f"Call Option Price: {bsm.call_option_price()}")
     print(f"Put Option Price: {bsm.put_option_price()}")
     ```

4. **Historical Volatility Calculation**
   - `calculate_historical_volatility(stock_data, window=252)`:
     - Computes **annualized historical volatility** from daily log returns.
     - Default `window=252` assumes ~252 trading days per year.
   - Example in the script:
     ```python
     jpm_volatility = calculate_historical_volatility(jpm_stock_data)
     print(f"JPM Historical Volatility: {jpm_volatility}")
     ```

5. **Greeks Calculation (via Inheritance)**
   - `BlackScholesGreeks(BlackScholesModel)` extends the base model with:
     - `delta_call()` – Δ of a call
     - `delta_put()` – Δ of a put
     - `gamma()` – Γ for both call/put
     - `theta_call()` – Θ of a call
     - `theta_put()` – Θ of a put
     - `vega()` – Vega (sensitivity to volatility)
     - `rho_call()` – ρ of a call
     - `rho_put()` – ρ of a put

   - Example usage in the script:
     ```python
     bsg = BlackScholesGreeks(S=100, K=100, T=1, r=0.05, sigma=0.2)
     print(f"Call Delta: {bsg.delta_call()}")
     print(f"Put Delta: {bsg.delta_put()}")
     ```

6. **Delta vs. Stock Price Plot**
   - The script computes call Delta over a range of stock prices and plots:
     - X-axis: stock price (80 to 120)
     - Y-axis: call Delta
   - Shows how Delta changes as the underlying moves **in-the-money** or **out-of-the-money**.

---

## Dependencies

This script uses the following Python libraries:

- `yfinance`
- `numpy`
- `scipy`
- `matplotlib`
- `mplfinance` *(imported but not used in the current script)*
- `plotly` *(imported but not used in the current script)*
- `datetime` (standard library)

### Install Requirements

You can install the required packages with:

```bash
pip install yfinance numpy scipy matplotlib mplfinance plotly
