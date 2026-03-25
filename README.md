# UK Climate Trend Analysis

An end-to-end statistical modelling project using long-run UK weather-station data to quantify warming trends, uncertainty, regional variation, and changing frost patterns.

## Overview

This project analyses long-run monthly weather data from 37 UK historic weather stations to examine how climate patterns have changed over time. The workflow combines exploratory climate analysis, trend-plus-seasonality regression, residual-bootstrap confidence intervals, August 2075 prediction intervals, and chi-square testing of frost occurrence.

The goal is not just to ask whether the UK has warmed, but to evaluate how consistently that signal appears across stations, where regional variation remains important, and how much uncertainty should be attached to station-level trend estimates and forecasts.

## Problem Statement

Climate change is often discussed at national or global scale, but long-run station-level data can reveal a more detailed picture of how warming and related weather patterns vary across locations.

This project investigates three linked questions:

- Do UK weather stations show consistent long-run warming in monthly maximum temperature?
- How much regional variation exists in warming rates and future August temperature scenarios?
- Has frost occurrence changed over time in a statistically detectable way across stations?

## Dataset

The analysis uses UK historic weather-station data based on the Met Office archive.

### Dataset characteristics

- 37 weather stations
- 39,427 monthly records
- coverage from 1853 to 2025
- station-level monthly variables including:
  - maximum temperature (`TMax`)
  - minimum temperature (`TMin`)
  - rainfall
  - frost days
  - sunshine
  - status flag
- station metadata including location and operating years

The project focuses mainly on:

- **TMax** for long-run warming analysis
- **Frost** for testing changes in frost occurrence over time
- **Rainfall** as a comparative variable in the exploratory analysis

## Project Pipeline

The overall workflow is:

- Load and combine station files with metadata
- Clean values and construct a continuous time index
- Add seasonal features using sine and cosine terms
- Explore long-run patterns in temperature, rainfall, and frost
- Fit station-level trend-plus-seasonality regressions
- Estimate warming-rate uncertainty using residual bootstrap
- Generate August 2075 prediction intervals
- Test frost-pattern change using chi-square contingency tables
- Compare results across all stations and interpret regional variation

## Exploratory Climate Patterns

The project begins with qualitative analysis of contrasting stations to identify visually meaningful long-run patterns.

### Temperature
Monthly maximum temperature shows the clearest long-run signal, with persistent warming visible across selected stations.

### Frost
Frost generally declines over time, reinforcing the warming signal and suggesting milder winter conditions at many stations.

### Rainfall
Rainfall is much more variable than temperature and does not exhibit one simple UK-wide long-run trend. Instead, local and regional differences appear to dominate.

## Modelling Approach

To estimate warming while accounting for the annual seasonal cycle, monthly maximum temperature is modelled using a linear trend term plus sine and cosine seasonal terms.

### Model form

For each station, monthly maximum temperature is modelled as:

```text
TMax = b0 + b1·t + b2·sin(ωt) + b3·cos(ωt) + ε
```

where:

t is a continuous time index
b1 is the long-run warming-rate parameter
the sine and cosine terms capture annual seasonality
ε is the remaining unexplained variation

The main parameter of interest is b1. A positive value indicates warming over time.

## Why Residual Bootstrap?

Standard regression output provides point estimates, but this project uses **residual bootstrap** to quantify uncertainty more explicitly.

The procedure is:

1. fit the trend-plus-seasonality regression
2. compute fitted values and residuals
3. resample residuals with replacement
4. generate new pseudo-series by adding resampled residuals back to fitted values
5. refit the model many times
6. use the resulting distribution of estimates to construct confidence intervals

The same logic is extended to prediction intervals by adding an additional resampled residual to future predictions.

## Key Results

### 1. Widespread warming across all stations
All stations show positive warming-rate estimates, and all 95% bootstrap confidence intervals lie above zero. This provides strong evidence of a geographically widespread rise in monthly maximum temperature across the full station network.

### 2. Warming is not uniform
The warming signal is clear, but the magnitude varies across stations. Southern and inland stations often warm faster than northern or strongly maritime stations, although the pattern is not explained by latitude alone.

### 3. Latitude explains part, not all, of the variation
Warming rate shows a weak-to-moderate negative relationship with latitude. More southerly stations tend to show larger warming rates, but local exposure, inlandity, urban influence, and record characteristics also appear to matter.

### 4. Frost-pattern change is more heterogeneous
Chi-square testing shows that frost occurrence has changed strongly at some stations, but not uniformly across all 37. This makes frost a useful complementary climate indicator rather than a direct substitute for continuous temperature-trend modelling.

## Example Forecasts

To make the trend estimates more interpretable, the project generates model-based prediction intervals for **August 2075**.

Example stations include:

- **Heathrow:** higher projected August maximum temperatures, consistent with stronger late-century warming
- **Lerwick:** lower projected August maximum temperatures, consistent with a cooler and more maritime climate

These forecasts are presented as **scenario-based model outputs**, not precise future weather predictions.

## Why This Project Matters

This project demonstrates more than basic statistical plotting. It shows the ability to:

- work with long-run real-world climate data
- build a consistent multi-station analytical workflow
- separate long-run trend from strong seasonal structure
- estimate uncertainty using bootstrap methods
- compare results across locations rather than relying on a single example
- interpret regional variation carefully without overclaiming
- communicate limitations and uncertainty clearly

That combination makes the project a strong example of applied statistical modelling and data-science reasoning.

## Limitations

This project is intentionally careful about the scope of its conclusions.

- The regression model is deliberately simple and does not capture every possible temporal feature.
- Station records differ in length, completeness, and local context.
- Rainfall patterns are highly variable and do not support one simple UK-wide summary.
- Frost-change significance is station-specific and depends on how binary frost occurrence is grouped over time.
- The analysis is observational and statistical rather than causal.

## Future Improvements

Several extensions could make the project more comprehensive:

- model minimum temperature alongside maximum temperature
- compare inland, coastal, northern, and southern station groups more formally
- test richer temporal models beyond linear trend plus annual seasonality
- add spatial visualisation of station-level warming rates
- extend prediction intervals to multiple months rather than August only
- investigate additional station-level covariates such as altitude or urban exposure

## Tech Stack

- Python
- NumPy
- pandas
- matplotlib
- SciPy
- statsmodels
- Jupyter Notebook

## How to Run

1. Clone this repository.
2. Install dependencies from `requirements.txt`.
3. Ensure the weather-station CSV files and metadata are available inside the `data/` directory.
4. Open `climate_trend_analysis.ipynb`.
5. Run the notebook cells in order to reproduce data loading, feature construction, modelling, bootstrap intervals, and frost-change testing.

## Repository Structure

```text
uk-climate-trend-analysis/
├── README.md
├── requirements.txt
├── data/
├── climate_trend_analysis.ipynb
├── .gitignore
└── LICENSE
