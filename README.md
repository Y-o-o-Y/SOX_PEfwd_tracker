# SOX Forward PE Tracker

An automated data pipeline that fetches daily fundamental metrics for SOX components, building PEfwd time series.

---

## ⚙️ Core Specifications

* **Data Source:** Finnhub API
* **Methodology:** Dynamic Market-Cap Weighted Average
* **Execution Schedule:** Automated daily via GitHub Actions at **5:00 PM EST** (configured via UTC cron).
* **Launch Date:** June 20, 2026

---

## 🗃️ Database Architecture (`sox_history.db`)

The SQLite database acts as a lightweight storage engine, splitting the ingested pipeline into two parallel time-series tables:

### 1. `portfolio_summary`
*Tracks the aggregated valuation metric of the entire semiconductor sector.*

| Column Name | Data Type | Key Type | Description |
| :--- | :--- | :--- | :--- |
| `date` | `TEXT` | PRIMARY KEY | The trading date (`YYYY-MM-DD`). |
| `weighted_pefwd` | `REAL` | - | The market-cap weighted Forward PE of the basket. |

### 2. `stock_daily_metrics`
*Tracks micro-level time series for each individual constituent.*

| Column Name | Data Type | Key Type | Description |
| :--- | :--- | :--- | :--- |
| `date` | `TEXT` | PRIMARY KEY | The trading date (`YYYY-MM-DD`). |
| `ticker` | `TEXT` | PRIMARY KEY | The stock's ticker symbol (e.g., `NVDA`). |
| `price` | `REAL` | - | Latest closing price (including after-hours final settlement). |
| `marketcap` | `REAL` | - | Total market capitalization fetched dynamically from Finnhub. |
| `pefwd` | `REAL` | - | Forward PE ratio based on consensus analyst expectations. |



## 🔬 Index Components (29 Tickers)
```json
[
  "NVDA", "AVGO", "AMD", "QCOM", "INTC", "MU", "TXN", "AMAT", "LRCX", "ADI",
  "KLAC", "MRVL", "MCHP", "MPWR", "ON", "NXPI", "ASML", "ARM", "SWKS", "QRVO",
  "SLAB", "ALGM", "DIOD", "CRUS", "TER", "MKSI", "ENTG", "COHR", "AMKR"
]

> 📌 **Note: TSM was removed because Finnbub returned an incorrect market cap (6 trillion).  June 20, 2026
```


<img width="518" height="363" alt="image" src="https://github.com/user-attachments/assets/41ecb7eb-b9e1-49f5-a399-dab930d27875" />
