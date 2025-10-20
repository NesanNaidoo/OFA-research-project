# Order Flow Analysis of Cryptocurrency Markets
![R](https://img.shields.io/badge/Made%20with-R-276DC3?style=flat-square&logo=r&logoColor=white)
![Quarto](https://img.shields.io/badge/Documented%20in-Quarto-4A154B?style=flat-square&logo=quarto&logoColor=white)
![License](https://img.shields.io/badge/License-Academic-lightgrey?style=flat-square)

A research project replicating and extending Silantyev (2019)'s order flow analysis of cryptocurrency markets.


## Authors

**Nesan Naidoo** and **Dominique Chetty**  
Statistics Honours Research Project, 2025

## Table of Contents
1. [Overview](#overview)
2. [Key Findings](#key-findings)
3. [Data](#data)
4. [Methodology](#methodology)
   - [Order Flow Metrics](#order-flow-metrics)
   - [Analysis Framework](#analysis-framework)
5. [Technical Notes](#technical-notes)
   - [Data Preprocessing](#data-preprocessing)
   - [OFI Calculation](#ofi-calculation)
   - [TFI Calculation](#tfi-calculation)
   - [Regression Model](#regression-model)
   - [Autocorrelation](#autocorrelation)
6. [Visualizations Generated](#visualizations-generated)
7. [Repository Structure](#repository-structure)
8. [Requirements](#requirements)
   - [Software](#software)
   - [Computational Requirements](#computational-requirements)
   - [R Packages](#r-packages)
9. [Usage](#usage)
   - [Data Extraction](#data-extraction)
   - [Running Analysis](#running-analysis)
   - [Key Functions](#key-functions)
   - [Stationarity Results](#stationarity-results)
10. [Key Results Summary](#key-results-summary)
11. [Comparison with Silantyev (2019)](#comparison-with-silantyev-2019)
12. [Limitations](#limitations)
13. [Future Research Directions](#future-research-directions)
14. [References](#references)
15. [Citation](#citation)
16. [Acknowledgments](#acknowledgments)


## Overview

This study examines how order book events influence cryptocurrency price movements through Order Flow Imbalance (OFI) and Trade Flow Imbalance (TFI) metrics.

**Research question**: Does Silantyev's finding that TFI better explains Bitcoin price movements than OFI remain valid in different market conditions?

**Approach**:
- **Replication**: Reproduce Silantyev (2019) analysis for October 1-23, 2017
- **Extension**: Apply same methodology to October 1-31, 2024
- **Assessment**: Evaluate generalisability of findings across different market periods

## Key Findings

**Replication period (October 2017)**:
- Both OFI and TFI significantly explain simultaneous mid-price changes
- TFI demonstrates stronger explanatory power than OFI
- Results consistent with Silantyev (2019)

**Extension period (October 2024)**:
- OFI explanatory power declines sharply
- TFI remains relatively more robust
- Evidence of structural changes in cryptocurrency market microstructure

**Implications**: Cryptocurrency market liquidity and order flow dynamics have evolved significantly since 2017.


## Data

**Source**: BitMEX XBTUSD perpetual contract  
**Type**: Level I quote and trade data

**Periods and size**:
- Replication: October 1-23, 2017 :  8.5 million quote updates and 4.1 million trades
- Extension: October 1-31, 2024 : 41.9 million quote updates and 2.0 million trades

**Access**: Raw data available in [Google Drive folder](https://drive.google.com/drive/folders/1lcg57o-UQn3S7LSUJV1Isf9N77JYSf2I?usp=sharing)

**Data structure**:
- **Quote data**: timestamp, bidPrice, bidSize, askPrice, askSize
- **Trade data**: timestamp, price, size, side



## Methodology

### Order Flow Metrics

**Order Flow Imbalance (OFI)**:
Measures changes in limit order book depth at bid and ask sides

**Trade Flow Imbalance (TFI)**:
Measures net signed volume of executed trades (buy minus sell volume)


### Analysis Framework

1. **Data preprocessing**: Clean and format timestamps, calculate mid-prices
2. **Metric calculation**: Compute OFI and TFI at multiple sampling intervals
3. **Time aggregation**: Intervals tested: 1s, 10s, 1min, 5min, 10min, 1hour
4. **Statistical testing**: 
   - Stationarity tests (ADF, KPSS, Phillips-Perron)
   - Autocorrelation analysis
5. **Regression analysis**: Linear models relating OFI/TFI to mid-price changes
6. **Comparative analysis**: R² comparison across periods and metrics


## Technical Notes

### Data Preprocessing

- Timestamps formatted to preserve microsecond precision
- Mid-price calculated as (bid + ask) / 2
- Missing values handled using last observation carried forward (LOCF)
- XBTUSD tick size: $0.10

### OFI Calculation

```
OFI = Σ[I(bid_price ≥ prev_bid) × bid_volume 
        - I(bid_price ≤ prev_bid) × prev_bid_volume
        + I(ask_price ≥ prev_ask) × prev_ask_volume
        - I(ask_price ≤ prev_ask) × ask_volume]
```

### TFI Calculation

```
TFI = Σ[signed_volume]
where signed_volume = volume (if Buy), -volume (if Sell)
```

### Regression Model

```
ΔP_k = α + β × Flow_k + ε
```

Where:
- ΔP_k = mid-price change over interval k
- Flow_k = OFI or TFI aggregated over interval k
- β = price impact coefficient

### Autocorrelation

10-second intervals show minimal autocorrelation for both metrics, validating use in regression models.

## Visualizations Generated

1. **Mid-price time series**: Intraday price movements
2. **Mid-price changes**: Tick-to-tick price variations
3. **Rolling volatility**: 1000-tick moving standard deviation
4. **ACF plots**: Autocorrelation for update arrivals, OFI, and TFI
5. **Scatter plots**: OFI/TFI vs mid-price changes at various intervals
6. **R² comparison plots**: Cross-period performance visualization
7. **Time series**: OFI and TFI evolution over study periods

## Repository Structure

- [OFA-Replication.qmd](./OFA-Replication.qmd): Replication analysis (Oct 2017)
- [OFA-Extension.qmd](./OFA-Extension.qmd): Extension analysis (Oct 2024)
- [OFA_Comparison_Plots.qmd](./OFA_Comparison_Plots.qmd): Supplementary visualization
- [_data_extraction_code/](./_data_extraction_code/): Data extraction scripts
  - [Data Extraction-Replication-Oct 2017.qmd](./_data_extraction_code/Data%20Extraction-Replication-Oct%202017.qmd)
  - [Data Extraction-Extension-Oct 2024.qmd](./_data_extraction_code/Data%20Extraction-Extension-Oct%202024.qmd)
- [_output/](./_output/) : all results obtained in the form of plots,tables, and raw test results.
  - [1. Replication - Oct 2017](./_output/1.%20Replication%20-%20Oct%202017)
  - [2. Extension - Oct 2024](./_output/2.%20Extension%20-%20Oct%202024)
- [_reference_papers](./_reference_papers/): key reference papers used for this study.

## Requirements

### Software
- RStudio 2024.09.0+375 "Cranberry Hibiscus" Release
- Quarto 1.5.57
- R 4.x or higher

### Computational Requirements
#### Replication period:
- **RAM**: Minimum 8GB
- **Processor**:	Intel(R) Core(TM) i5-6300U CPU @ 2.40GHz   2.50 GHz
- **Disk Space**: Minimum 1GB
- **Operating system**: Windows
- **System type**: 64-bit operating system, x64-based processor

#### Extension period:
- **RAM**: Minimum 16GB
- **Processor**:	13th Gen Intel(R) Core(TM) i5-13500T   1.60 GHz
- **Disk Space**: Minimum 3GB
- **Operating system**: Windows
- **System type**: 64-bit operating system, x64-based processor

### R Packages

```r
# Data manipulation
library(dplyr)
library(tidyr)
library(data.table)

# Time series
library(lubridate)
library(zoo)
library(xts)

# Statistical testing
library(tseries)
library(urca)
library(lmtest)

# Visualization
library(ggplot2)
library(patchwork)

# Utilities
library(knitr)
library(stringr)
library(readr)
library(readxl)
```

Install all dependencies:

```r
install.packages(c("dplyr", "tidyr", "data.table", "lubridate", 
                   "zoo", "xts", "tseries", "urca", "lmtest",
                   "ggplot2", "patchwork", "knitr", "stringr",
                   "readr", "readxl"))
```

## Usage

### Data Extraction

**For replication period (2017)**: [Data Extraction-Replication-Oct 2017.qmd](./_data_extraction_code/Data%20Extraction-Replication-Oct%202017.qmd)

```r
# Downloads data from BitMEX S3 bucket
source("_data_extraction_code/Data Extraction-Replication-Oct 2017.qmd")
# Outputs: XBTUSD_quotes_20171001_20171023.csv
#          XBTUSD_trades_20171001_20171023.csv
```

**For extension period (2024)**: [Data Extraction-Extension-Oct 2024.qmd](./_data_extraction_code/Data%20Extraction-Extension-Oct%202024.qmd)

```r
# Reads compressed local data files
source("_data_extraction_code/Data Extraction-Extension-Oct 2024.qmd")
# Outputs: XBTUSD_quotes_20241001_20241031.csv
#          XBTUSD_trades_20241001_20241031.csv
```

### Running Analysis

**Replication study**: [OFA-Replication.qmd](./OFA-Replication.qmd)

```r
# Open OFA-Replication.qmd in RStudio
# Render to PDF
quarto::quarto_render("OFA-Replication.qmd")
```

**Extension study**: [OFA-Extension.qmd](./OFA-Extension.qmd)

```r
# Open OFA-Extension.qmd in RStudio
# Render to PDF
quarto::quarto_render("OFA-Extension.qmd")
```

### Key Functions 

Functions used in both: [OFA-Replication.qmd](./OFA-Replication.qmd) and [OFA-Extension.qmd](./OFA-Extension.qmd)

**aggregate_ofi(df, interval, tick_size)**
- Aggregates quote data to specified time interval
- Computes OFI and mid-price changes
- Returns data frame with interval_time, OFI, mid_price_change

**aggregate_tfi(trades, quotes, interval, tick_size)**
- Aggregates trade data to specified time interval
- Computes TFI (signed volume) and mid-price changes
- Returns data frame with interval_time, TFI, mid_price_change

**run_ols(df, xvar, yvar)**
- Performs OLS regression
- Returns alpha, beta, t-statistic, R², and significance level

### Stationarity Results

Both OFI and TFI series tested stationary across all intervals using:
- Augmented Dickey-Fuller (ADF) test
- KPSS test
- Phillips-Perron (PP) test

## Key Results Summary

### R² Comparison (Explanatory Power)

**Replication Period (2017)**:

| Interval | OFI R² | TFI R² |
|----------|--------|--------|
| 1 sec    | 7.40%  | 13.32% |
| 10 sec   | 40.97% | 37.53% |
| 1 min    | 55.43% | 58.56% |
| 5 min    | 52.09% | 68.95% |
| 10 min   | 49.76% | 70.74% |
| 1 hour   | 51.31% | 75.16% |

**Extension Period (2024)**:

| Interval | OFI R² | TFI R² |
|----------|--------|--------|
| 1 sec    | 25.21% | 12.06% |
| 10 sec   | 20.86% | 11.98% |
| 1 min    | 8.76%  | 16.22% |
| 5 min    | 0.84%  | 20.73% |
| 10 min   | 0.03%  | 21.91% |
| 1 hour   | 0.96%  | 27.48% |

Code available in :  [OFA_Comparison_Plots.qmd](./OFA_Comparison_Plots.qmd)

<img width="496" height="306" alt="Overall R squared comparison" src="https://github.com/user-attachments/assets/220a5558-4521-41e6-9213-2de73b82859f" />


## Comparison with Silantyev (2019)

### Similarities
- Both studies use BitMEX XBTUSD data
- Same order flow metrics (OFI, TFI)
- Comparable sampling intervals tested
- Linear regression framework

### Differences
- Extended study period (Oct 2024 vs Oct 2017)
- Additional stationarity tests (Phillips-Perron)
- Comparative analysis across time periods
- Focus on temporal stability of findings

## Limitations

- Limited to single cryptocurrency (Bitcoin)
- Single exchange (BitMEX)
- Two snapshot periods (potential sampling bias)
- Linear models only (no nonlinear relationships explored)
- No intraday pattern controls
- Does not account for external market events

## Future Research Directions

- Multi-cryptocurrency analysis
- Cross-exchange comparison
- Longer time horizons
- Nonlinear and machine learning models


## References

**Primary reference**:
Silantyev, E. (2019). Order flow analysis of cryptocurrency markets. *Digital Finance*, 1(1), 191-218.


## Citation

If you use this code or findings, please cite:

```
Naidoo, N., & Chetty, D. (2025). Order Flow Analysis of Cryptocurrency Markets. Statistics Honours Research Project, 
University of Cape Town.
```


## Acknowledgments

- Andrew Paskaramoorthy for guidance and supervision on this project.
- Silantyev (2019) for foundational methodology.
- BitMEX for providing historical market data.
