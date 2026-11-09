# CRM & CM Coursework 1: Market Analysis and Stock Segmentation Using RFM

Module 6BUIS017, University of Westminster. Individual coursework (CRM Case Study).

## What this project does
Segments S&P 500 shares in two ways and compares them:
1. Risk and return: mean weekly return and weekly beta (against ^GSPC).
2. Trading activity: adapted RFM from daily volume (Recency, Frequency, Monetary).

Both are clustered with Ward agglomerative clustering and K-Means.

## Key facts
- Study period: 1 Jan 2021 to 1 Jan 2025 (end date exclusive)
- Ticker list: Wikipedia, retrieved 2026-11-08 (503 shares)
- Price data: yfinance 0.2.66, pandas 2.2.3 (daily Close, Adj Close, Volume, plus ^GSPC)
- Observation date for RFM: 2024-12-31 (last common trading date)
- Valid shares after validation: 486 (17 excluded, see `data/processed/exclusion_log.csv`)

## Repository layout
| Path | Contents |
|---|---|
| `CW1.ipynb` | Main notebook (open in Google Colab with the badge at the top) |
| `data/raw/` | Unedited downloads: tickers, adj_close, close, volume (cached snapshots) |
| `data/processed/` | exclusion_log.csv, valid_stocks.csv, feature_table.csv, cluster assignments |
| `outputs/` | Charts and dendrograms |

## How to run
1. Open `CW1.ipynb` in Google Colab (Runtime > Run all).
2. If Yahoo Finance is unavailable, use the cached-data cell in section 1.1 (it loads the files in `data/raw/` from this repository).

## Use of generative AI
Generative AI (Claude) was used for explanation, planning, code support and drafting, as permitted by the module leader. All results were run and checked by the author.
