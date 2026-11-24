# I used OpenAI ChatGPT and Codex to assist with application development, coding, debugging, financial methodology, testing, and documentation.

# The analysis and final investment decision are my own. This project is for educational purposes only and is not investment advice.

# Investment committee memo — Meridian Foundation reserve

## Decision and allocation range — pending Mark’s approval

No final recommendation or allocation range has been approved by Mark. The current application result does not support an allocation because the constrained low-volatility portfolio fails the mandate’s 25% annual-turnover limit without approval. The committee should not treat any portfolio as approved until Mark selects a decision and allocation range after resolving or accepting the documented conditions.

## Evidence

| Portfolio | Realized annualized return | Realized volatility | Sharpe ratio | Annual rebalance turnover | Mandate status |
|---|---:|---:|---:|---:|---|
| Equal weight | 6.57% | 3.78% | 0.69 | 0.0% | No hard failure; objective, loss, and horizon remain review items |
| Risk parity | 4.17% | 0.34% | 0.61 | 14.9% | No hard failure; objective, loss, and horizon remain review items |
| Constrained low-volatility | 7.97% | 4.11% | 0.98 | 35.0% | Fails annual turnover; objective, loss, and horizon remain review items |
| 60/40 benchmark | 11.78% | — | — | — | Comparison reference, not an allocation authorization |

The comparison covers November 2024 through September 2026, uses a 36-month rolling estimation window, annual rebalancing, and 25-basis-point one-way base costs. The constrained portfolio’s expected return estimate is 6.57% and estimated risk is 4.66%; both are model estimates, not forecasts.

## Stress and changed-input evidence

The higher-cost changed-input experiment left constrained weights, expected return estimate, and estimated risk unchanged. It reduced realized annualized return from 7.97% to 7.68%, reduced Sharpe ratio from 0.98 to 0.93, and increased total modeled costs from 0.34% to 0.88%. The annual-turnover failure remained.

The interactive stress panel recalculates weights under return, volatility, correlation, window, cost, and experimental-constraint changes. It stops on invalid inputs, infeasibility, hard mandate failures, or unresolved allocation-critical terms.

## Risks, monitoring, and stop conditions

- **Increase only after:** Mark approves the decision and range; optimizer target, loss measurement, and breach response are defined; turnover becomes compliant or receives approval; and longer validation is completed.
- **Reduce or stop:** any hard mandate breach, stale/invalid data, infeasible stressed portfolio, or change to mandate/universe.
- **Rebalance:** annual cadence only after an approved live target exists; escalate before a projected turnover breach.
- **Monitor:** monthly data-quality review and annual portfolio/mandate review.

## Key limitations and committee ask

The backtest is 23 months, while the mandate horizon is seven years and the objective is assessed over five-year periods. The source lacks publication timestamps, no point-in-time CPI series is included, and the calendar-loss rule lacks a defined return measure and breach response. The four selected funds are an implementation universe, not the complete Lab 13 permitted universe.

Mark must independently select Adopt, Allocate to, Rebalance, Pilot, or Reject and approve an allocation range before this memo becomes the authoritative final recommendation. See `validation-and-ai-use.md`, `LOCKED_PREDICTION.md`, and `CHANGED_INPUT_RESULT.md` for validation and experiment evidence.
