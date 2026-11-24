# I used OpenAI ChatGPT and Codex to assist with application development, coding, debugging, financial methodology, testing, and documentation.

# The analysis and final investment decision are my own. This project is for educational purposes only and is not investment advice.

# Meridian Foundation investment-management application

## Purpose and current decision status

This Streamlit application compares transparent portfolio rules, screens them against the Lab 13 Meridian Foundation reserve mandate, tests assumptions, and records a controlled changed-input experiment. It is a course project, not a live investment system.

No final committee recommendation or allocation range has been approved by Mark. The constrained low-volatility portfolio has the strongest realized risk-adjusted result in the supplied test but fails the 25% annual-turnover limit without approval. The application displays an active stop condition and does not authorize an allocation.

## Investment mandate

- Seven-year horizon; annual rebalancing; CPI + 4% per year net of costs over rolling five-year periods.
- Benchmark: 60% global equities and 40% U.S. investment-grade bonds.
- Long-only; no borrowing, short sales, or derivatives; 8% maximum non-exempt issuer; 25% annual-turnover cap without approval; 90% redeemable within five business days; 3% reserve-liquidity minimum.
- Calendar-year loss cannot exceed 10%, but Lab 13 does not define the return measure or breach response.

See [data/mandate_source.md](data/mandate_source.md) for the full source record.

## Install and launch

Tested with Python 3.14.7. From this folder in PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
streamlit run app.py
```

Open the address Streamlit prints, normally `http://localhost:8501`. To run checks:

```powershell
python -m pytest -q
```

Dependencies: Streamlit, pandas, Plotly, NumPy, and pytest; see `requirements.txt`.

## Navigation

- **Executive Overview:** decision status, allocation status, compliance, expected return, estimated risk, benchmark comparison, and stops.
- **Investment Mandate:** approved rules and unresolved terms.
- **Portfolio Comparison:** equal weight, risk parity, constrained low-volatility, allocations, cumulative performance, drawdown, risk-return, and mandate screens.
- **Investment Signal and Data Card:** point-in-time 12-1 momentum ranking and provenance.
- **Stress Testing:** interactive estimate, window, cost, and experimental-constraint tests.
- **Locked Changed-Input Record:** prediction and actual result side by side.
- **Committee Recommendation:** decision framework; only Mark may approve a final selection.

## Data and portfolio methodology

The implementation universe is SGOV, IEI, SPIB, and IVV. `data/prices.csv` contains 60 monthly Yahoo Finance adjusted-close observations from October 2021 through September 2026, downloaded October 22, 2026. Adjusted closes incorporate distributions and splits; see [data/fund_sources.md](data/fund_sources.md). Historical data are not forecasts.

All portfolios use a 36-month rolling estimation window, annual rebalances, November 2024–September 2026 out-of-sample returns, and a 25-basis-point one-way base cost.

- **Equal weight:** 25% in each fund.
- **Risk parity:** iterative weights that approximately equalize estimated risk contribution.
- **Constrained low-volatility:** Lab 15 five-percentage-point grid search subject to long-only, reserve, and the Lab 14 6.5% CPI teaching proxy. This proxy is not an approved replacement for the actual CPI-linked objective or missing optimizer target.

Annualized return equals average monthly return times 12; annualized volatility equals monthly standard deviation times the square root of 12. Sharpe uses SGOV monthly return as the risk-free proxy. Maximum drawdown is the largest decline from a prior cumulative-value peak.

## Signal, stress, and changed-input record

12-1 momentum equals adjusted close at t-2 months divided by adjusted close at t-14 months, minus 1. It excludes the most recent month and filters out rows dated after the selected decision date. It is informational only and does not alter weights.

Stress testing recalculates the constrained portfolio after individual expected-return, volatility, correlation, lookback, cost, and experimental-constraint changes. Experimental constraints do not replace the original mandate. The process stops on invalid inputs, infeasibility, hard failures, or unresolved allocation-critical terms.

The locked prediction is [LOCKED_PREDICTION.md](LOCKED_PREDICTION.md), with recorded pre-experiment hash `c803d21bf77dc4e2e714e196be90910a98e44481`. The single change was one-way cost from 25 to 65 basis points. Results are in [CHANGED_INPUT_RESULT.md](CHANGED_INPUT_RESULT.md) and [outputs/changed-input-transaction-cost-2026-11-23.md](outputs/changed-input-transaction-cost-2026-11-23.md). The remote commit content could not be read from this environment, so independent remote verification remains a limitation.

## Limitations and troubleshooting

The out-of-sample record is 23 months, shorter than the mandate horizon and rolling five-year objective. No point-in-time CPI series is supplied; the calendar-loss measure and breach response are undefined; the optimization target is unspecified; prices lack vintage timestamps; and the constrained portfolio fails the turnover cap.

- **`streamlit` is not recognized:** activate `.venv`, then rerun `python -m pip install -r requirements.txt`.
- **Missing package:** activate `.venv` and reinstall requirements.
- **Price-data validation error:** confirm `data/prices.csv` exists with ordered `date,SGOV,IEI,SPIB,IVV` columns and positive values.
- **Stress STOP:** restore base inputs or revise the scenario; do not treat an invalid/infeasible result as allocation-ready.
- **Git unavailable:** use GitHub Desktop or GitHub web; never revise the pre-experiment locked prediction.
