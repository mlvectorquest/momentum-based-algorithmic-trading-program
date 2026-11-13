# Momentum-Based Algorithmic Trading Program

A first attempt at a momentum-based trading strategy for the S&P 500 index, built as a Udacity learning project. The strategy calibrates a Geometric Brownian Motion (GBM) model to historical prices, forecasts the price 10 days ahead, and uses the forecast and its expected shortfall to size a position. It is backtested over one year (2020-06-01 to 2021-05-31).

## Contents

| File | Description |
| --- | --- |
| `Project_Learner.ipynb` | Notebook with the full implementation, the backtest and the wealth plot |
| `SP500.csv` | Daily S&P 500 closing prices (tab-separated: `theday`, `price`), from 2019-01-02 |
| `SP500.db` | SQLite database with the `prices` and `positions` tables |

## How it works

1. **`GBM` class**: simulates price paths and calibrates drift (`mu`) and volatility (`sigma`) from a price series.
2. **`prepare()`**: loads `SP500.csv` into the `prices` table and creates and initialises the `positions` table with the starting cash.
3. **`analyse(which_day)`**: calibrates the GBM on the prior 252 trading days and forecasts the price 10 days ahead. It computes a confidence interval and the expected shortfall.
4. **`position_size(which_day, forecast, ES)`**: turns the forecast and expected shortfall into a target number of units.
5. **`main(begin_on)`**: loops over each trading day in the backtest, trades, and records each position in the `positions` table.

## Database schema

```sql
CREATE TABLE prices (theday TEXT PRIMARY KEY, price REAL);
CREATE TABLE positions (time_of_trade TEXT, symbol TEXT, quantity REAL, cash REAL);
```

## Running it

Requires Python 3 with `numpy`, `scipy`, `matplotlib` and `jupyter`.

```bash
pip install numpy scipy matplotlib jupyter
jupyter notebook Project_Learner.ipynb
```

Run the notebook top to bottom. `prepare()` rebuilds the `prices` and `positions` tables, and `main('2020-06-01')` runs the backtest. The last cells plot total wealth (`quantity * price + cash`) over the backtest period.

## Disclaimer

This is an educational project, not financial advice. Backtest results do not predict future performance.
