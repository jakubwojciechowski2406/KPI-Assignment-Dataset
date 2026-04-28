# KPI-Assignment-Dataset
KPI assessment of NASDAQ stock data — coursework assignment
# NASDAQ Stock Data — Data Card

**Author:** Jakub Wojciechowski 49872

This is the Data Card for the dataset I built for the assignment on data quality assessment. The dataset contains daily closing prices for 5 NASDAQ companies, downloaded from Yahoo Finance, and is evaluated using the four KPIs discussed in class: Completeness, Latency, Accuracy, and Consistency.

The full code is in `nasdaq_data_quality.ipynb`.

---

## 1. Source of data

The data comes from **Yahoo Finance** and was downloaded with the Python library **`yfinance`**. For each company I used `yf.Ticker(symbol).history(start=..., end=..., interval="1d")` to get the daily close prices, and then saved them to a CSV file.

Each CSV has two columns:

| Column | Description |
|---|---|
| `Date` | Trading day, format `YYYY-MM-DD` |
| `Close` | Closing price in USD |

5 NASDAQ companies were chosen, each with a different time period (as required by the task):

| Ticker | Company    | Period    | Start       | End         | Rows |
|--------|------------|-----------|-------------|-------------|-----:|
| AAPL   | Apple      | 12 months | 2025-04-28  | 2026-04-28  | 251  |
| MSFT   | Microsoft  | 9 months  | 2025-07-29  | 2026-04-28  | 188  |
| NVDA   | NVIDIA     | 6 months  | 2025-10-28  | 2026-04-28  | 124  |
| AMZN   | Amazon     | 3 months  | 2026-01-28  | 2026-04-28  |  62  |
| TSLA   | Tesla      | 11 months | 2025-05-28  | 2026-04-28  | 230  |

---

## 2. KPIs

The four KPIs were calculated for each dataset, exactly as implemented in the notebook.

**Completeness** — checked with `df.info()` and `df["Close"].isnull().sum()`. Result for every company: **0 missing values**.

**Latency** — calculated as `(today - df.index.max()).days`. Result for every company: **1 day** (the most recent date in each dataset is 2026-04-27, with `today` being 2026-04-28 when the notebook was run).

**Accuracy** — checked by counting negative and zero prices with `(df["Close"] < 0).sum()` and `(df["Close"] == 0).sum()`. Result for every company: **0 negative prices, 0 zero prices**.

**Consistency** — checked by printing `df.columns.tolist()` and `df.dtypes` for each file (after setting `Date` as index). All 5 files have the same column `Close` with the same data type `float64`.

### Summary table

| Ticker | Rows | Missing Close | Latency (days) | Negative prices | Zero prices |
|--------|-----:|--------------:|---------------:|----------------:|------------:|
| AAPL   | 251  | 0             | 1              | 0               | 0           |
| MSFT   | 188  | 0             | 1              | 0               | 0           |
| NVDA   | 124  | 0             | 1              | 0               | 0           |
| AMZN   |  62  | 0             | 1              | 0               | 0           |
| TSLA   | 230  | 0             | 1              | 0               | 0           |

### Basic statistics for the `Close` column (from `df.describe()`)

| Ticker | Mean (USD) | Std    | Min    | Max    |
|--------|-----------:|-------:|-------:|-------:|
| AAPL   | 243.42     | 27.27  | 194.68 | 285.92 |
| MSFT   | 465.93     | 51.38  | 356.77 | 539.83 |
| NVDA   | 186.00     |  8.81  | 165.17 | 216.61 |
| AMZN   | 220.72     | 18.39  | 198.79 | 263.99 |
| TSLA   | 390.91     | 52.30  | 284.70 | 489.88 |

### Extra: ADF test (bonus from class example)

The Augmented Dickey-Fuller test was run on the `Close` series for each company, like in the class example. The notebook prints only the ADF statistic:

| Ticker | ADF Statistic |
|--------|--------------:|
| AAPL   | -1.31         |
| MSFT   | -1.00         |
| NVDA   | -1.86         |
| AMZN   | -0.11         |
| TSLA   | -1.55         |

This is a bonus from the class example and is not one of the four required KPIs.

---

## 3. Conclusion

Based on the four KPIs calculated in the notebook, the data quality is good across all 5 companies:

- **Completeness:** 0 missing values in any dataset.
- **Latency:** 1 day for every company.
- **Accuracy:** 0 negative and 0 zero prices for every company.
- **Consistency:** all 5 files have the same column (`Close`) and the same data type (`float64`).
