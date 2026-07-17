# Methodology

## Data
- Source: ONS, CPIH Annual Rate (series L55O)
- Sample: January 2005 onward (excludes 1989–2004 backward-modelled estimates, 
  which ONS itself flags as lower-quality "official statistics" rather than 
  National Statistics)

## Stationarity
- ADF test on levels: p = 0.138 → non-stationary
- ADF test on first difference: p < 0.001 → stationary
- Confirms d = 1 for ARIMA

## Model order
- Tested ARIMA(3,1,1) and ARIMA(1,1,1)
- ARIMA(3,1,1): AIC=123.7, BIC=141.4, but ar.L2 not significant (p=0.92)
- ARIMA(1,1,1): AIC=127.3, BIC=137.9, all terms significant
- Selected ARIMA(1,1,1) — BIC favors it, and avoids an unnecessary parameter
- Note: ACF/PACF showed a spike at lag 12/24, suggesting seasonality not 
  captured by this model — flagged as a limitation, candidate for SARIMA later

  ## Initial evaluation: 52-month-ahead forecast
- Test period: Feb 2022–May 2026 (coincides with the energy price shock)
- Naive: RMSE=2.308, MAE=1.977
- ARIMA(1,1,1): RMSE=2.322, MAE=2.149
- ARIMA did not beat naive over this long single-shot horizon — likely because 
  a 52-month-ahead forecast flattens toward the unconditional mean and cannot 
  anticipate the 2022 shock, while naive happened to track similarly by chance
- This motivates a more realistic evaluation: short-horizon rolling forecasts 
  (1-month, 3-month ahead), re-estimated as new data arrives, rather than one 
  long static forecast

  ## Rolling one-month-ahead evaluation
- Rolling forecast: refit ARIMA(1,1,1) at each step using only data available 
  up to that point, forecast 1 month ahead, repeat across the test period
- Rolling Naive: RMSE=0.506, MAE=0.352
- Rolling ARIMA: RMSE=0.475, MAE=0.369
- Mixed result: ARIMA achieves 6% lower RMSE (fewer large errors), but 5% 
  higher MAE (slightly less accurate in typical months)
- Interpretation: ARIMA's autoregressive term appears to help it react ahead 
  of larger moves, at a small cost to average accuracy in calm periods
- Some individual monthly refits triggered convergence warnings; results 
  appear stable across the test period regardless