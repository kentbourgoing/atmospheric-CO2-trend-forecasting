# The Keeling Curve: Atmospheric CO₂ Trend Forecasting

A time series forecasting analysis that predicted atmospheric CO₂ levels would reach 420 ppm by 2030 using 1997 data—but validation against actual measurements revealed CO₂ crossed this threshold eight years earlier in 2022, demonstrating how climate change is outpacing historical models and highlighting the critical importance of regularly updating forecasts with new data.

---

## Problem and Goal

- **Problem:** Climate scientists and policymakers need accurate long-term forecasts of atmospheric CO₂ concentrations to inform climate policy and assess environmental risks. However, time series models trained on historical data may not capture accelerating trends, and their accuracy degrades over long forecasting horizons. Understanding how well models generalize to unseen future data is critical for determining when forecasts need recalibration.
- **Why It Matters:** Atmospheric CO₂ is the primary driver of global warming, and crossing critical thresholds like 420 ppm has implications for climate tipping points, international climate agreements (e.g., Paris Agreement), and long-term mitigation strategies. Inaccurate forecasts can lead to delayed policy responses or misallocation of resources for emissions reduction.
- **Goal:** Build time series forecasting models using 40 years of Mauna Loa Observatory CO₂ data (1958-1997) to predict when atmospheric CO₂ would reach 420 ppm and 500 ppm thresholds. Then validate these predictions against actual observed data through 2024 to assess model accuracy, compare forecasting approaches, and quantify how actual CO₂ growth has deviated from historical projections.

---

## Approach

1. **Exploratory Data Analysis (EDA):** Analyzed 468 monthly CO₂ observations (1958-1997) using ACF/PACF plots, seasonal decomposition (classical additive vs. STL multiplicative), and moving average smoothers to identify strong upward trend, annual seasonal cycles (photosynthesis-driven), and non-stationarity requiring differencing.

2. **Polynomial Trend Modeling with Seasonality:** Tested 7 polynomial trend models (linear, quadratic, 2nd-4th order) with log transformation and seasonal dummy variables, evaluated using AIC/BIC/AICc, residual diagnostics, and Ljung-Box tests to assess white noise assumptions.

3. **ARIMA Model Development:** Applied Augmented Dickey-Fuller unit root test to confirm non-stationarity, performed first-order and seasonal differencing to achieve stationarity, then fit multiple ARIMA specifications including ARIMA(0,1,0)(0,1,0), ARIMA(0,1,1)(1,1,0), ARIMA(1,1,1)(1,1,0), and ARIMA(0,1,3)(1,1,0).

4. **Model Selection and Validation:** Selected ARIMA(0,1,0)(0,1,1)[12] as optimal model based on lowest AIC/BIC, white noise residuals (Ljung-Box p > 0.05), and normal residual distribution. Compared against log-polynomial 3rd order model with seasonality as alternative approach.

5. **Threshold Forecasting with Confidence Intervals:** Generated forecasts through 2100 using selected ARIMA model, computed 95% confidence intervals via bootstrapping, identified first/last times when CO₂ would cross 420 ppm and 500 ppm thresholds using upper/lower CI bounds and mean estimates.

6. **Out-of-Sample Validation (1998-2024):** Evaluated 1997-trained models against actual NOAA weekly CO₂ data (1998-2024), computed forecast accuracy metrics (RMSE, MAE, MAPE), and compared predicted vs. observed threshold crossing dates to quantify forecast error.

7. **Model Retraining with Modern Data:** Retrained ARIMA models on extended dataset (1974-2024) with weekly granularity, tested seasonally-adjusted (SA) vs. non-seasonally-adjusted (NSA) versions, performed 2-year train/test split, and generated updated 100-year forecasts to 2122.

8. **Comparative Model Analysis:** Evaluated model generalization by comparing 1997 model predictions to reality, discovering simpler models (ARIMA(0,1,0)(0,1,0), Poly 2 Seasonal) outperformed more complex models on out-of-sample data, revealing overfitting in original model selection.

---

## Results

### Technical Deliverables
- **1997 Historical Models:** Tested 11 model configurations (7 polynomial trend + 4 ARIMA specifications) on 468 monthly observations (1958-1997), selected ARIMA(0,1,0)(0,1,1)[12] and Log-Polynomial 3rd order with seasonality as best performers based on AIC/BIC and residual diagnostics.
- **420 ppm Threshold Forecast (1997 Model):** Predicted CO₂ would reach 420 ppm by April 2030 (mean estimate), with 95% CI ranging from April 2016 (earliest) to beyond 2100 (latest), and projected 2100 levels at 557 ppm (95% CI: 377-820 ppm).
- **Out-of-Sample Validation Accuracy:** Evaluated 1997 models on 312 months of actual data (1998-2024), achieving RMSE of 0.0056 (Poly 2 Seasonal) and 0.0035 (ARIMA 0,1,0)(0,1,0), revealing simpler models generalized better than originally-selected complex models.
- **Modern ARIMA Models (2024 Training Data):** Retrained on 2,600+ weekly observations (1974-2024), learning optimal parameters ARIMA(0,1,4) with drift (NSA) and ARIMA(1,1,5) (SA), achieving in-sample RMSE of 0.0069 and out-of-sample RMSE of 0.0112 on 2-year test set.
- **Updated 100-Year Forecasts:** Projected CO₂ to reach 420 ppm by late 2022, 500 ppm first time around 2040s, last sub-420 ppm observation around 2092, and 2122 mean estimate of 580 ppm (95% CI: 435-774 ppm) using modern ARIMA(0,1,4) model.

### Key Outcomes
- **Accelerating CO₂ Growth:** Actual CO₂ levels crossed 420 ppm in 2022, **eight years earlier** than the 1997 model's mean forecast (2030), demonstrating that CO₂ growth is outpacing historical trends and highlighting limitations of long-term forecasts with changing system dynamics.
- **Model Generalization Insights:** Simpler models (ARIMA(0,1,0)(0,1,0), Poly 2 Seasonal) outperformed more complex models (ARIMA(0,1,0)(0,1,1), Poly 3 Seasonal) on out-of-sample data, revealing overfitting in 1997 model selection and emphasizing importance of parsimony in time series forecasting.
- **Widening Uncertainty Over Time:** Confidence intervals expanded dramatically for long-term forecasts (2100: 377-820 ppm range), growing from ~85 ppm width at 20-year horizon to ~400 ppm at 100-year horizon, quantifying fundamental uncertainty in century-scale climate predictions.
- **Forecast Recalibration Impact:** Retraining with 27 additional years of data (1997-2024) shifted 420 ppm threshold prediction from 2030 to 2022 (actual) and narrowed near-term uncertainty, demonstrating value of regular model updates for maintaining forecast reliability in non-stationary systems.

### Model Performance Comparison

**Linear Models Tested on 1998-2024 Observed Data:**

![Linear Model Forecasts vs. Observed CO2](Plots/Forecast%20of%20Monthly%20Mean%20CO2%20Levels%20from%201998%20to%202022%20-%20Linear.png)

Comparison of polynomial trend models trained on 1997 data evaluated against actual CO₂ measurements from 1998-2024. The Poly 2 Seasonal model (green) tracks observed data (red) more closely than the Poly 3 Seasonal model (blue), which was originally selected as "best" based on 1997 AIC/BIC criteria. This demonstrates that the simpler 2nd-order polynomial better captured the underlying trend and generalized more effectively to new data, revealing that the 3rd-order model had overfit to historical patterns. The divergence illustrates how CO₂ growth exceeded even the best 1997 predictions.

**ARIMA Models Tested on 1998-2024 Observed Data:**

![ARIMA Model Forecasts vs. Observed CO2](Plots/Forecast%20of%20Monthly%20Mean%20CO2%20Levels%20from%201998%20to%202022%20-%20ARIMA.png)

Comparison of ARIMA models trained on 1997 data evaluated against actual CO₂ measurements from 1998-2024. The simpler ARIMA(0,1,0)(0,1,0) model (green) outperformed the more complex ARIMA(0,1,0)(0,1,1) model (blue) that was originally selected based on white noise residuals and lower AIC/BIC. The simpler model's forecast aligns more closely with observed data (red), achieving lower RMSE (0.0035 vs. 0.0211) and demonstrating better generalization. This counterintuitive result highlights that models optimized for in-sample fit may not perform best on out-of-sample data, especially when the underlying system dynamics are changing.

---

## Tech/Methods

**Languages & Frameworks:** R, tidyverse, tsibble, fable, feasts, ggplot2

**Statistical Tools:** Time series analysis (ACF/PACF, STL decomposition), ARIMA/SARIMA modeling, Augmented Dickey-Fuller test, Ljung-Box test, AIC/BIC/AICc information criteria

**Data Sources:** R built-in `co2` dataset (1958-1997 monthly), NOAA Global Monitoring Laboratory (1974-2024 weekly via API)

**Methods:** Seasonal differencing, log transformation for variance stabilization, polynomial trend regression with seasonal dummy variables, time-weighted aggregation, bootstrap confidence intervals, train/test split validation, seasonal adjustment (STL decomposition)

---

## Repo Structure

```
keeling-curve-forecasting/
├── Rmd File/                     # Main analysis workflow
│   └── The Keeling Curve.Rmd    # Complete R Markdown analysis (EDA, modeling, forecasting)
│
├── Report/                       # Final deliverables
│   └── Lab_2_FINAL.pdf          # Compiled PDF report with all results and visualizations
│
├── Plots/                        # Key visualization outputs
│   ├── Forecast of Monthly Mean CO2 Levels from 1998 to 2022 - Linear.png
│   └── Forecast of Monthly Mean CO2 Levels from 1998 to 2022 - ARIMA.png
│
└── README.md                     # Project documentation
```

---

## Prerequisites

**Required Software:**
- **R (≥ 4.0.0)** - Statistical computing environment
- **RStudio** (recommended) - IDE for R development

**Required R Packages:**
Install via `install.packages(c("tidyverse", "tsibble", "fable", "feasts", "tseries", "slider", "knitr", "kableExtra", "latex2exp", "patchwork", "gridExtra"))`

- `tidyverse` - Data manipulation and visualization
- `tsibble` - Time series data structures
- `fable` - Forecasting models (ARIMA, TSLM)
- `feasts` - Time series feature extraction and visualization
- `tseries` - Augmented Dickey-Fuller test
- `slider` - Moving average calculations
- `knitr`, `kableExtra` - Report generation and table formatting
- `latex2exp`, `patchwork`, `gridExtra` - Advanced plotting

**Data Sources:**
- **Historical CO₂ Data (1958-1997):** R built-in `co2` dataset (automatically available)
- **Modern CO₂ Data (1974-2024):** NOAA Global Monitoring Laboratory - automatically fetched via permalink in code

---

## Notes: Limitations and Next Steps

### Current Limitations:

- **Long-Term Forecast Uncertainty:** 100-year forecasts (2100, 2122) have extremely wide confidence intervals (~400 ppm range), limiting actionable insights for century-scale planning. ARIMA models assume stationary dynamics after differencing, which may not hold over multi-decadal periods.
- **Changing System Dynamics:** CO₂ emissions are driven by policy decisions, technological innovation, and economic activity—factors not captured in univariate time series models. Actual 2022 data showed acceleration beyond 1997 predictions, indicating regime shifts that pure statistical models cannot anticipate.
- **Model Selection Overfitting:** Models selected based on in-sample AIC/BIC (ARIMA(0,1,0)(0,1,1), Poly 3 Seasonal) underperformed simpler alternatives on out-of-sample validation. The analysis lacked robust cross-validation (e.g., rolling window, expanding window) to detect overfitting earlier.
- **Seasonal Adjustment Trade-Offs:** Seasonally-adjusted (SA) models showed minimal accuracy gains over non-seasonally-adjusted (NSA) models despite added preprocessing complexity. The analysis did not thoroughly compare STL vs. classical decomposition vs. X-13ARIMA-SEATS seasonal adjustment methods.

### Next Steps:

- **Multivariate Forecasting:** Incorporate exogenous predictors like global GDP, fossil fuel consumption, renewable energy adoption, and policy indicators (carbon pricing, emissions targets) using ARIMAX or Vector Autoregression (VAR) models to capture drivers of CO₂ growth.
- **Ensemble Methods:** Combine polynomial trend, ARIMA, exponential smoothing (ETS), and machine learning models (LSTM, Prophet) via weighted averaging or stacking to improve forecast robustness and reduce reliance on single-model assumptions.
- **Regime-Switching Models:** Implement Markov-Switching ARIMA or structural break detection (Chow test, CUSUM) to identify and model transitions between different CO₂ growth regimes (e.g., pre-2000 vs. post-2000 acceleration).
- **Scenario-Based Forecasting:** Develop multiple forecast trajectories based on IPCC emission scenarios (SSP1-1.9 net-zero, SSP2-4.5 moderate, SSP5-8.5 high-emission) to provide decision-makers with range of plausible outcomes rather than single-point predictions.
- **Bayesian Time Series Models:** Use Bayesian structural time series (BSTS) or state-space models to quantify parameter uncertainty, incorporate prior knowledge about CO₂ dynamics, and generate probabilistic forecasts with more interpretable uncertainty bounds.
- **Real-Time Dashboard:** Build interactive Shiny dashboard that automatically fetches latest NOAA data, updates forecasts monthly, displays threshold crossing probabilities, and alerts when observed data deviates significantly from predictions to enable adaptive model recalibration.

---

## Data Sources and Context

**Data Sources:**
- **Historical CO₂ Data (1958-1997):** R built-in `co2` dataset, monthly mean atmospheric CO₂ concentrations (ppm) measured at Mauna Loa Observatory, Hawaii. Digitized from original Keeling Curve measurements.
  - Citation: Keeling, C.D., Scripps Institution of Oceanography
- **Modern CO₂ Data (1974-2024):** NOAA Global Monitoring Laboratory, weekly mean atmospheric CO₂ concentrations via API permalink (`https://gml.noaa.gov/webdata/ccgg/trends/co2/co2_weekly_mlo.csv`)
  - Data retroactively adjusted to X2019 calibration scale (2017, 2021 updates)
  - Usage: Academic/research purposes under NOAA data policy

**Project Context:**
- **Course:** UC Berkeley School of Information - DATASCI 271 (Statistical Methods for Data Science)
- **Duration:** August 2024 - December 2024 (12-week academic project)
- **Team Members:**
  - Kelly Short
  - Kent Bourgoing
  - Anshul Zutshi
  - Changhao Meng

---

## Team Members

| Name | Email | LinkedIn |
|------|-------|----------|
| Kent Bourgoing | kent1bp@berkeley.edu | [LinkedIn](https://www.linkedin.com/in/kent-bourgoing/) |
| Kelly Short | short.kelly@ischool.berkeley.edu | - |
| Anshul Zutshi | anshulzutshi@ischool.berkeley.edu | [LinkedIn](https://www.linkedin.com/in/anshul-zutshi123/) |
| Changhao Meng | chm@ischool.berkeley.edu | [LinkedIn](https://www.linkedin.com/in/chenghaomeng/) |
