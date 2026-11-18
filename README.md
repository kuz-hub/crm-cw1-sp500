# CRM & CM Coursework 1 — Market Analysis and Stock Segmentation Using RFM

Module **6BUIS017W**, University of Westminster. Individual coursework (CRM Case Study).

Everything in this repository is produced by one notebook, **`CRM_CW1.ipynb`**
([open in Colab](https://colab.research.google.com/github/kuz-hub/crm-cw1-sp500/blob/main/CRM_CW1.ipynb)).
`Runtime > Run all` reproduces every table, figure and data file below.

## What this project does

S&P 500 shares are segmented in **two separate ways** and the two segmentations are compared:

1. **Risk and return** — mean weekly return and weekly beta against `^GSPC`.
2. **Trading activity** — adapted RFM built from daily volume (Recency, Frequency, Monetary).

Each feature matrix is clustered with **Ward agglomerative clustering** and with **K-Means**,
giving the four baseline solutions the brief asks for, plus a controlled sensitivity run with
Monetary removed.

## Headline facts for this run

| | |
|---|---|
| Study period | 1 Jan 2021 to 1 Jan 2025, **end date exclusive** (1,005 trading sessions, 4 Jan 2021 – 31 Dec 2024) |
| Ticker list | Wikipedia *List of S&P 500 companies*, snapshot retrieved **2026-11-08** (503 symbols) |
| Price source | Yahoo Finance via **yfinance**, `auto_adjust=False` (keeps both `Close` and `Adj Close`) |
| Benchmark | `^GSPC` |
| Observation date (RFM) | **2024-12-31** — the final common completed trading date |
| Shares retained | **486** of 503 (17 excluded — see `data/processed/inclusion_exclusion_log.csv`) |
| Aligned weekly observations | **207** per share, identical week labels for shares and benchmark |
| Event rule | volume > **2 ×** the mean volume of the preceding **20** sessions, the tested day excluded |
| F and M window | the latest **60** trading sessions ending on the observation date |
| Chosen cluster counts | k = **5** (risk and return), k = **4** (trading activity) |
| Reproducibility | random seed 42, `n_init=20`; every window, threshold and seed is set in one configuration cell |

All of these values are written to `data/processed/report_headline_numbers.csv` by the notebook.

## Repository layout

| Path | Contents |
|---|---|
| `CRM_CW1.ipynb` | The whole analysis: Task 1, Task 2 and the Task 3 evidence |
| `data/raw/` | Unedited snapshots: ticker list, `adj_close_daily.csv`, `close_daily.csv`, `volume_daily.csv` |
| `data/processed/` | Inclusion and exclusion log, acquisition log, feature table, cluster assignments, cluster and sector profiles, correlation table, headline numbers |
| `data/mart/` | The Task 3 data mart, materialised as a star schema (4 dimensions, 4 fact tables) |
| `outputs/` | Figures: feature distributions, dendrograms, k-selection, cluster plots, sector composition, RFM correlations, Monetary-removal sensitivity, star schema diagram |

### Task 3 data mart (`data/mart/`)

| Table | Grain |
|---|---|
| `dim_date` | one row per trading date |
| `dim_share` | one row per ticker, with sector and inclusion status |
| `dim_benchmark` | one row per benchmark series |
| `dim_calculation_run` | one row per run: provenance, windows, threshold, seed, package versions |
| `fact_daily_market_rfm_window` | share × trading date (the 60-session RFM window) |
| `fact_weekly_return` | share × Friday week-ending date, with the aligned benchmark return |
| `fact_share_features` | share × run — the five derived features |
| `fact_cluster_assignment` | share × clustering solution × run |

Every fact row carries `Run_id`, so re-running with a different observation date, window length
or threshold **adds** rows rather than overwriting them, and any number in the report can be
traced back to the configuration that produced it.

## How to run

1. Open `CRM_CW1.ipynb` in Google Colab and choose **Runtime > Run all**.
2. The notebook first tries to read the ticker list from Wikipedia and the prices from Yahoo
   Finance. **If either live source is unavailable it falls back automatically** to the
   preserved snapshot in `data/raw/`, and prints which source was actually used — no cell has
   to be edited by hand. The committed outputs in this repository were produced from that
   documented snapshot.
3. All result files are written to `data/processed/`, `data/mart/` and `outputs/`.

Package versions used for the committed run are printed in the first cell of the notebook and
stored in `data/mart/dim_calculation_run.csv`.

## Scope and limitations

The clusters are **descriptive segments of past behaviour**, not predictions. RFM here describes
share trading activity, not company value, customer loyalty or investment profitability;
Monetary is a liquidity proxy, not market capitalisation; and weekly beta differences alone do
not establish portfolio diversification, because dependence between share returns is not
measured. The constituent list is current while the price history is historical, so the sample
carries **survivorship bias**. These points are set out in sections 1.5, 2.5 and 3.2.3 of the
notebook.

## Use of generative AI

Generative AI tools were used for explanation, planning and code support. All analysis
decisions, results and checks are the author's own.
