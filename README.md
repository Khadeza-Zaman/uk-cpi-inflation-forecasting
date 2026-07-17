# UK CPIH Inflation Forecasting

This project tests whether ARIMA can beat a naive forecast for UK CPIH 
inflation. I used ONS data from 2005 onward and ran two different tests: a 
long static forecast and a rolling one-month-ahead forecast. ARIMA loses to 
naive over the long horizon, but wins on RMSE in the rolling test.

## Headline result

| Forecast | ARIMA RMSE | Naive RMSE | ARIMA MAE | Naive MAE | Winner (RMSE) |
|---|---|---|---|---|---|
| 52-month-ahead | 2.322 | 2.308 | 2.149 | 1.977 | Naive |
| Rolling 1-month-ahead | 0.475 | 0.506 | 0.369 | 0.352 | ARIMA |

Over 52 months, both forecasts flattened out and missed the 2022 energy 
price shock completely. Over a rolling 1-month horizon, ARIMA had a lower 
RMSE than naive, though naive still had a slightly lower MAE — ARIMA seems 
to avoid the worst misses but isn't more accurate on average in calm months.

## Charts

**52-month-ahead forecast:**
![52-month forecast comparison](results/figures/arima_vs_naive_52month.png)

**Rolling 1-month-ahead forecast:**
![Rolling forecast comparison](results/figures/rolling_forecast_comparison.png)

Full methodology and evaluation details are in [`docs/methodology.md`](docs/methodology.md).

## Data

UK CPIH Annual Rate (ONS series L55O), January 2005 to present. I left out 
1989–2004 since ONS classifies that stretch as backward-modelled estimates 
rather than the official National Statistic series.

## How to reproduce

1. Clone this repo
2. Create a virtual environment and run `pip install -r requirements.txt`
3. Open `notebooks/01_exploration.ipynb` and run all cells

## Project structure

```
data/raw          - original ONS CSV
notebooks/        - main analysis notebook
results/figures/  - saved plots
docs/             - full methodology write-up
```

## Limitations

- ACF/PACF showed a seasonal pattern (spikes at lag 12 and 24) that plain 
  ARIMA doesn't capture. SARIMA would be the next step.
- The 52-month test period happens to land on the 2022 energy shock, so 
  the error there is probably worse than it would be in a calmer stretch.