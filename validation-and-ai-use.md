# AI disclosure and ownership

Codex was used to help organize the repository, draft application code and documentation, and identify validation steps. Mark owns the analysis, assumptions, recommendation, and final investment decision.

# Educational purpose

Education only, not investment advice.

# Validation and AI-use record

## Current validation status

- 2026-11-23: the walk-forward application and stress page passed Streamlit tests without an application exception; `python -m pytest -q` reported ten passing tests. The checks cover price-data shape, full long-only weights, the 23-month out-of-sample boundary, unresolved-mandate blocking, 12-1 momentum no-look-ahead, invalid stress-input stops, and covariance-stress recalculation. See `../outputs/validation-2026-11-23.md` for the actual command and output.

## Methodology boundary

- Data: supplied monthly Yahoo Finance adjusted closes for SGOV, IEI, SPIB, and IVV, October 2021–September 2026.
- Backtest: rolling 36-month historical estimation; annual rebalance; realized out-of-sample months from November 2024 through September 2026.
- Costs: one configurable one-way rate applied consistently to all portfolios; 25 basis points is the initial displayed value, not a sourced mandate input.
- Risk parity: iterative equal risk-contribution weights from the pre-rebalance covariance matrix.
- Constrained search: Lab 15 5% grid search for minimum volatility subject to long-only, 3% reserve, and the Lab 14 6.5% CPI teaching proxy. This is not an approved mandate optimization target.
- A clean-user cold run, actual visible output, and final decision consistency check remain outstanding.

## Locked Changed-Input Record

The unchanged human-authored prediction is [LOCKED_PREDICTION.md](../LOCKED_PREDICTION.md). The executed 25-to-65-basis-point one-way cost result is [CHANGED_INPUT_RESULT.md](../CHANGED_INPUT_RESULT.md) and [the output record](../outputs/changed-input-transaction-cost-2026-11-23.md). The supplied commit hash is `c803d21bf77dc4e2e714e196be90910a98e44481`; remote commit-content verification was unavailable in this environment.

## Material AI interactions

The current working log is [ai-use-log.md](ai-use-log.md). Mark must review and finalize each disposition before submission.
