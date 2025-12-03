# 📘 Future-Proofing TSA Operations: Data-Driven Resource Optimization
## Time Series Forecasting • Workforce Optimization • TSA Passenger Throughput Modeling
### 📖 Overview

This project develops a data-driven passenger forecasting and workforce optimization model for the U.S. Transportation Security Administration (TSA). Using historical passenger throughput data, advanced time series modeling, and operational workforce assumptions, this analysis provides:

* Accurate monthly passenger forecasts

* A robust ARIMA-based model selected through multi-stage evaluation

* A workforce requirement model converting forecasted passengers → required screening officers

* Actionable recommendations for transitioning TSA from reactive to proactive staffing

The goal is to reduce wait times, improve workload balance, and enable flexible staffing aligned with predictable demand cycles.

### 📘 Project Background

TSA screens ~2.8M passengers daily across 47,500 Transportation Security Officers (TSOs). Despite predictable seasonality in travel demand, staffing often relies on annual schedules, leading to:

* Inconsistent wait times

* Inefficient checkpoint capacity use

* Idle staffing during low-volume periods

* Strain during peak travel months

This project implements time series forecasting to align staffing with actual demand—reducing unnecessary costs and improving passenger experience.

### 📊 Data

Source: TSA Daily Passenger Throughput
Format: Daily records aggregated to monthly totals
Key columns:

* Date (daily)

* Numbers (daily passenger throughput)

* Aggregated to Total (monthly passengers)

The dataset spans pre-pandemic, pandemic, and recovery periods—allowing evaluation under stable and volatile conditions.

### 🔧 Methods & Modeling Pipeline

The project follows a structured workflow:

1. Data Cleaning & Transformation

* Convert daily data → monthly totals using yearmonth().

* Remove final incomplete month.

* Add covid_shock dummy (Feb 2020–Dec 2021).

* Validate missingness, outliers, and date ranges.

2. Exploratory Analysis

* Time series plotting of monthly volumes

* Outlier analysis

* STL decomposition (trend + seasonal components)

* Identification of:

  * Summer peaks (Jun–Aug)

  *  Winter troughs (Jan–Feb, September)

  * Overall CAGR ≈ 1.15% since 2019

3. Training & Test Split

* Training: Through 2023

* Testing: 2024–2025 horizon

* Forecast horizon: 12–14 months

4. Candidate Models

* Tested models include:

* Exponential Smoothing (ETS)

* ARIMA on raw values

* ARIMA on log-transformed values

* ARIMA with COVID dummy & trend/season terms

* Manual ARIMA configurations:

(0,1,1)(0,1,1)

(1,1,0)(1,1,0)

(0,1,1)(0,1,0)

Others tested via ACF/PACF guidance

5. Model Diagnostics

* Residuals vs fitted

* KPSS stationarity tests

* AICc comparison

* Hold-out set accuracy

* Full rolling-origin cross-validation using stretch_tsibble()

### 🧠 Model Comparison & Selection
Key Findings:

* Log-transformed models performed worse (heteroscedastic residuals & higher RMSE).

* ETS performed well on a single holdout year but poorly in cross-validation.

* ARIMA models proved more stable across volatile periods, especially the pandemic years.

### 🎯 Final Model

ARIMA(1,1,0)(1,1,0)
Selected for:

Lowest test-set RMSE (≈ 5.7 million)

Best cross-validation stability

Strong residual diagnostics

Captures both seasonal and trend components effectively

### 📈 Forecasting Results

The final ARIMA model forecasts monthly passenger volumes through 2026:

* Expected peak months (Jun–Aug 2026) near 98 million passengers

* Forecast includes a 10% operational safety buffer to accommodate uncertainty

* Seasonality remains consistent with prior years, offering reliable advance planning signals.

### 👷 Workforce Optimization Model

To translate forecasts into staffing requirements, TSA operational assumptions were used:

Parameter	Value
Lane throughput	180 pax/hour
Lane operating hours	16 hrs/day
TSOs needed per lane	~8
Active TSO workforce	~23,530 (50% screening duty)
Passengers supported per agent/month	~10,625
Workforce Outputs:

For each forecasted month (w/ 10% buffer), the model computes:

Required screening agents

% of available workforce needed

Key Insight:

Only 25–36% of screeners are needed to maintain <30-minute wait-time levels—even during peak periods.

This reveals substantial unused capacity that can be reallocated or scheduled more intelligently.


### 🔁 How to Reproduce the Analysis

Clone the repository:
```
git clone <your-repo-url>
cd tsa-forecasting
```

Ensure Data/TSA.xlsx exists in the expected directory.

Render the Quarto file:
```
quarto render tsa_analysis.qmd
```

This will reproduce all figures, models, tables, forecasts, and workforce calculations.

### 🧾 Results Summary

* TSA passenger throughput shows strong seasonality and predictable annual peaks.

* The ARIMA(1,1,0)(1,1,0) model provides the most robust, accurate, and stable forecasts.

* Actual screening demand uses only 40% of available workforce.

* With forecasting, TSA can:

* Shift from yearly → monthly scheduling

* Reduce overstaffing in low-volume months

* Improve staff satisfaction through predictable shifts

* Maintain <30-minute wait times while reducing strain

* Reallocate staff to training, admin roles, or surge support

### 🎯 Recommendations

**Adopt a monthly forecasting cadence** using the ARIMA model.

**Align staffing schedules** to monthly demand rather than annual templates.

**Reallocate excess workforce capacity** during low-volume months.

**Pilot the model operationally** during holiday travel periods.

**Integrate forecasting into checkpoint scheduling tools** for 2027+ deployment.

Forecasting-based staffing represents a major opportunity to reduce inefficiencies without cutting headcount.

### 👥 Contributors

Bart Teeuwen

Jason Avalos 

