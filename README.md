# DMP-05: From a Trading Idea to a Factor Portfolio
Hello Worl

Welcome. This folder has everything you need for the DMP-05 session: four Jupyter notebooks,
the data they use, and the small Python modules they import.

You'll start with one trading rule on one stock and end with a factor portfolio built on
the Nifty 50. Along the way you'll see why a single backtest can mislead, what trading costs
do to a strategy, how diversification cuts risk, and how beta can be used to build a
portfolio.

The worked examples use Indian stocks and the Nifty 50, but none of the ideas are specific to
India. Backtesting, trading costs, diversification and beta work the same way in any market,
and the notebooks are written so you can swap in stocks from wherever you trade (see
[Using a different market](#using-a-different-market)).

> **Keep every file in this folder together.** The notebooks import the `.py` modules and
> read the data files from the same folder. If you move a notebook somewhere else on its own,
> it will stop working.

---

## What's in the folder

### Notebooks (work through them in this order)

| File | Topic |
|------|-------|
| `DMP_05_code_01_Idea_to_Backtest.ipynb` | Build and backtest an EMA crossover with an ADX filter on one stock |
| `DMP_05_code_02_Portfolio_Diversification.ipynb` | Run the same rules on 50 stocks and combine them into a portfolio |
| `DMP_05_code_03_Beta.ipynb` | Understand beta and estimate it with a regression |
| `DMP_05_code_04_Betting_Against_Beta.ipynb` | Build a low-beta portfolio, then add a quality (ROE) screen |

### Data

| File | What it is |
|------|-----------|
| `DMP_05_data_nifty50_ohlc_prices.csv` | Daily Open, High, Low, Close for the 50 stocks and the Nifty 50 index (`^NSEI`), from January 2013 |
| `DMP_05_data_nifty50_constituents_roe.csv` | The 50 stocks with company name, industry and ROE (as of January 2018) |
| `DMP_05_data_nifty50_constituents_oct2017.pdf` | The official NSE Indices (IISL) list of Nifty 50 constituents on 31 October 2017. This is the source of the stock universe. |

### Python modules

The modules are kept short and readable, so open them if you want to see how something works.

| File | What it does |
|------|-------------|
| `data.py` | Loads prices, either live from Yahoo Finance or from the bundled CSV |
| `alpha.py` | The EMA crossover and ADX indicators |
| `performance_analytics.py` | Turns signals into returns, charges costs, and calculates Sharpe, drawdown, the monthly table and the trade log |
| `plots.py` | Chart styling shared by all notebooks |

---

## Before you start

**Packages.** The notebooks use pandas, numpy, plotly, statsmodels and yfinance (plus
nbformat, so Plotly charts display in Jupyter). You need **pandas 2.2 or newer** and a recent
yfinance, because older versions of both are missing features the code uses. The notebooks
were tested with Python 3.11, pandas 2.2, numpy 1.24, plotly 6.0, statsmodels 0.14 and
yfinance 0.2.55.

**Running.** Open the notebooks from inside this folder, in Jupyter or VS Code, and run each
one from top to bottom with `Shift` + `Enter`. Cells depend on the ones above them, so don't
skip ahead. If something looks wrong, use **Kernel > Restart & Run All** to start clean.

---

## The learning path

### Notebook 1: From a Trading Idea to a Backtest

You'll build a momentum strategy on LIC Housing Finance, step by step: check the data, write
two alphas (EMA crossover and ADX), combine them into a position, backtest it, then add
realistic trading costs and read the trade log.

**Look out for:** why `.shift(1)` matters, and how much of the return disappears once each
trade pays around 0.24% in charges.

*Needs internet:* prices download live from Yahoo Finance, so your numbers will differ slightly
from the recording.

### Notebook 2: The Same Strategy But A Different Answer

The exact same rules, run on all 50 stocks. The results range from clearly negative to quite
good, even though nothing in the rules changed. Then you combine all 50 into one
equal-weight portfolio and see its volatility drop by about two thirds.

**Look out for:** the formula that predicts portfolio volatility from the average correlation,
and how few stocks you need to get most of the benefit.

*No internet needed:* uses the bundled CSV.

### Notebook 3: Understanding and Calculating Beta

A short notebook on what beta means and how to estimate it: turn prices into returns, plot
the stock against the market, and fit a regression. The slope is beta.

**Look out for:** why Nestle India's beta comes out well below 1.

*Needs internet:* prices download live from Yahoo Finance.

### Notebook 4: Betting Against Beta

CAPM says higher beta should earn higher returns. You'll rank the Nifty 50 by beta using
data up to 2017, hold the low-beta stocks from 2018 onward, and see what happened. Then you'll
add a quality filter using ROE.

**Look out for:** why the beta is measured on one period and the performance on another, and
whether the quality filter actually helped.

*No internet needed:* uses the bundled CSV.

Each notebook ends with a **Further reading** or **References** list of research papers, if
you want to go deeper.

---

## Assumptions used across the notebooks

- **Stock universe.** The 50 stocks are the Nifty 50 as of 31 October 2017, taken from the
  NSE Indices sheet in this folder. We use that historical list, not today's, so stocks that
  later struggled or left the index are still included.
- **Corporate actions since 2017.** HDFC Ltd merged into HDFC Bank in July 2023, and after that
  date Notebook 4 uses HDFC Bank's returns. Tata Motors demerged in October 2025 and its prices
  are adjusted for it. Bharti Infratel now trades as Indus Towers, and Indiabulls Housing
  Finance as Sammaan Capital.
- **Trading costs.** Every market charges you to trade through some mix of commission, taxes or
  duties, exchange and regulator fees, and the bid-ask spread. The mix and the total differ from
  market to market. Notebook 1 uses about 0.12% on each buy and each sell (about 0.24% per round
  trip), which is roughly the official charges on an Indian equity delivery trade with
  commission assumed to be zero. Slippage isn't included. If you trade elsewhere, change
  `COST_BPS` to match your market's charges. Notebooks 2 and 4 leave costs out to keep the focus
  on portfolio construction.
- **Sharpe ratio.** Annualised average daily return divided by annualised standard deviation.
  No risk-free rate is subtracted, to keep things simple.
- **Prices.** Stock prices are adjusted for splits and dividends. The Nifty 50 benchmark
  (`^NSEI`) is a price index, so it doesn't include dividends.

---

## Using a different market

The methods don't change from one market to another. Only the tickers and the cost figure do.

- **Notebooks 1 and 3** download prices live, so you can point them at any stock and index
  that Yahoo Finance carries. Change `INSTRUMENT` in Notebook 1, or `STOCK` and `MARKET` in
  Notebook 3. For example: `AAPL` and `^GSPC` (S&P 500) in the US, `HSBA.L` and `^FTSE`
  (FTSE 100) in the UK, or `7203.T` and `^N225` (Nikkei 225) in Japan.
- **Notebook 1's costs.** Set `COST_BPS` to the per-side cost of trading in your market.
- **Notebooks 2 and 4** read the bundled Nifty 50 file. To run them on another universe,
  download prices for your own list of stocks (for example with `data.get_stock_data`) and
  save them in the same layout as `DMP_05_data_nifty50_ohlc_prices.csv`. Use the index members
  as they stood at the start of your test period, not today's list.

---

## Troubleshooting

**`ModuleNotFoundError: No module named 'data'`** (or `alpha`, `plots`, and so on)
The notebook can't find the `.py` files. Make sure you opened it from inside this folder and
that the modules are sitting next to it.

**`ModuleNotFoundError: No module named 'yfinance'`** (or `plotly`, `statsmodels`)
A package is missing. Install it with `pip install <package>`. If you use more than one Python
environment, install into the one your notebook kernel uses.

**Notebook 1 or 3 fails with an empty download, `IndexError`, or "Rate limited"**
Yahoo Finance sometimes limits how often you can download, especially when many people in a
class run the notebook at once. Wait a few minutes and run the cell again. Once a download
succeeds, it's saved in a `data_cache` folder, so later runs that day won't need to download
again.

**Charts don't appear**
Make sure `nbformat` is installed, then restart the kernel. In VS Code, check
that the Jupyter extension is up to date.

**The `data_cache` folder**
Notebooks 1 and 3 create it to store downloads. You can delete it at any time. It will be
rebuilt the next time you run them.

---

## Risk disclaimer

These notebooks are for learning. The strategies are simplified teaching examples, not
investment advice or recommendations. Backtested results are hypothetical and depend on the
period, data and assumptions chosen. Past or backtested performance doesn't guarantee future
results. Trading and investing involve risk, including the possible loss of capital.

---

## Need help?

All notebooks and data files are available on the LMS. If you can't find something, or run
into a problem not covered above, prefix your query with **Support** or write to
**support@quantinsti.com**.
