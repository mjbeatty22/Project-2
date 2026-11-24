# Changed-Input Result — post-run record

**Status:** Executed 2026-11-23. The original locked prediction is preserved separately and unchanged.

| Field | Actual executed result |
|---|---|
| Date and time of run | 2026-11-23 23:16:05 -05:00 |
| Locked-prediction commit hash and URL | `c803d21bf77dc4e2e714e196be90910a98e44481`; `https://github.com/mjbeatty22/Project-2/commit/c803d21bf77dc4e2e714e196be90910a98e44481` |
| Selected input changed | One-way transaction cost only |
| Original value and units | 25 basis points one-way |
| Changed value and units | 65 basis points one-way (the locked prediction spells this “on-way”; Mark confirmed it means one-way) |
| Portfolio weights | No change: 2024-11-01 SGOV 55%, IEI 0%, SPIB 0%, IVV 45%; 2025-11-01 SGOV 85%, IEI 0%, SPIB 5%, IVV 10%. |
| Portfolio risk | Estimated risk unchanged at 4.66%. Realized annualized volatility changed from 4.11% to 4.03%. |
| Expected return | Estimated return unchanged at 6.57%. Realized annualized return decreased from 7.97% to 7.68%. |
| Mandate-compliance result | Unchanged: annual turnover was 35.0%, above the 25.0% limit without approval (FAIL); the objective, loss rule, and horizon remain REVIEW. |
| Comparison with pre-run prediction | Mixed: estimated risk remained unchanged; weights did not move closer to existing weights because they did not change; expected return estimate did not decrease, while realized net return decreased; the portfolio remained noncompliant because of turnover. |
| Decision effect | No allocation approval. The changed cost does not cure the existing turnover failure or unresolved mandate terms. |
| Failure diagnosis, if prediction differed | Transaction costs are applied after the optimizer selects weights in `src/backtest.py`. Therefore, changing only transaction cost does not alter expected-return inputs, covariance inputs, or target weights; it reduces realized net returns through larger deductions at rebalances. |
| Command / application path used | `src.backtest.walk_forward('Constrained low-volatility', config=StressConfig(transaction_cost_bps=25))` versus the identical call with `transaction_cost_bps=65`. |
| Output artifact link | `outputs/changed-input-transaction-cost-2026-11-23.md` |
