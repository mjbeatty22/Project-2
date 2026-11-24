# Final audit — Meridian Foundation investment-management application

**Audit date:** 2026-11-23  
**Overall status:** **Needs revision — not complete for submission.**

This is a strict audit of the current repository and running application. It does not alter Mark's locked prediction or make a committee decision. The calculation checks use the supplied local data only; they do not validate the economic accuracy or completeness of Yahoo Finance data.

## Verification performed

- Created a separate temporary Python environment, installed only `requirements.txt`, ran `python -m pytest -q`, and started the application with `streamlit run app.py`.
- Automated tests: **10 passed** in the clean environment.
- Streamlit view smoke test: **passed** for all seven navigation sections without an application exception.
- HTTP startup smoke test: **passed** (`streamlit run app.py --server.headless true --server.port 8513` returned HTTP 200).
- Independently recomputed the constrained portfolio's documented two target periods directly from `data/prices.csv`, including 25-basis-point initial and 35-basis-point rebalance turnover costs. The displayed return (7.965632%), volatility (4.114039%), Sharpe (0.977805%), maximum drawdown (-2.821714%), benchmark return (11.782307%), tracking error (5.296276%), total costs (0.3375%), and 100% weight sums agree with the application.

`git` is not installed or available in this environment, and the supplied GitHub commit could not be accessed here. Therefore, Git history and remote-file immutability cannot be independently verified.

## Eight mandatory requirements

| # | Requirement | Status | Evidence / location | Remaining deficiency | Recommended correction |
|---:|---|---|---|---|---|
| 1 | A stranger can run a functioning Python web app from the README. | **PASS** | [README.md](README.md); [app.py](app.py); `requirements.txt`; clean-environment test and HTTP smoke test above. | The README gives a PowerShell-specific activation command, which is acceptable but does not include a non-PowerShell alternative. | Optional: add a `cmd.exe` activation alternative. |
| 2 | Original Lab 13 mandate is retained and used. | **PARTIAL** | [data/mandate_source.md](data/mandate_source.md); Investment Mandate page; [README.md](README.md). Horizon, objective, payouts/reserve, benchmark wording, long-only rules, concentration, turnover, liquidity, and loss condition are recorded. | The visible app/README mandate summary does not fully state the $100m reserve and $0.75m quarterly payout context. The implementation universe is a four-ETF proxy, not the full eligible Lab 13 universe. | Display the full source mandate in the app, including reserve size and payout amount; label the ETF list consistently as a limited implementation universe rather than the mandate's complete universe. |
| 3 | Equal weight, second baseline, and constrained portfolio with transparent calculations and rule verdicts. | **PARTIAL** | Portfolio Comparison page; [src/backtest.py](src/backtest.py); [src/committee.py](src/committee.py); independent constrained-result recomputation above. | Equal weight and risk parity are present and weights sum to 100%. The constrained candidate is *not constrained by the 25% annual-turnover mandate cap* and produces a 35% breach. The benchmark implementation is 60% IVV / 40% SPIB, but IVV is U.S. equity, not global equity. Portfolio turnover also uses prior target weights instead of drifted pre-rebalance weights, so the turnover test is a simplified estimate. | Do not call the candidate mandate-constrained until all hard constraints are enforced. Add a turnover constraint based on pre-trade/drifted weights, or return an explicit infeasible result and stop allocation. Replace IVV with a documented global-equity proxy or label the benchmark comparison as a non-mandate U.S.-equity proxy. |
| 4 | One transparent point-in-time investment signal with a data card. | **PARTIAL** | Investment Signal and Data Card page; [src/signal.py](src/signal.py); [tests/test_signal.py](tests/test_signal.py). | The code strictly filters rows dated after the decision date and the future-row mutation test passes. However, the price file has no vendor publication timestamps, so true information availability cannot be proven; it is a conservative assumption. | Retain the conservative convention and obtain/download data with documented availability/vintage timestamps if a fully verified point-in-time claim is required. |
| 5 | Stress tests for estimates, windows, costs, and constraints, with stop conditions. | **PARTIAL** | Stress Testing page; `StressConfig`, `validate_config`, and `walk_forward` in [src/backtest.py](src/backtest.py); [tests/test_backtest.py](tests/test_backtest.py). | The UI covers each required category, exports results, shows original-mandate verdicts, and stops invalid/infeasible scenarios. Tests cover invalid bounds and estimate-risk recalculation. It does not test data staleness, and the hard turnover rule is detected after optimization rather than imposed during it. | Add a data-as-of freshness rule and a test. Enforce hard mandate constraints inside optimization, then rerun base and stress results under the corrected model. |
| 6 | Locked Changed-Input Record was committed before experiment and original prediction remains unchanged. | **FAIL** | [LOCKED_PREDICTION.md](LOCKED_PREDICTION.md); [CHANGED_INPUT_RESULT.md](CHANGED_INPUT_RESULT.md); application record page. | The post-run record names hash `c803d21bf77dc4e2e714e196be90910a98e44481`, but the authoritative locked file's commit-hash field is blank. With no local Git executable and no accessible remote commit, this audit cannot verify commit order or prove the local prediction equals the pre-experiment committed version. | In GitHub, open that exact commit, confirm it contains the exact locked prediction, and paste the immutable commit URL/hash into the locked record **only if this is a new explanatory record that does not edit Mark's prediction content**. Preserve the original committed blob and include a screenshot/link in the submission manifest. Do not rerun or alter the experiment. |
| 7 | Committee recommendation with allocation range and monitoring triggers. | **FAIL** | Committee Recommendation page; [docs/decision-memo.md](docs/decision-memo.md); [src/committee.py](src/committee.py). | The evidence framework, possible actions, monitoring triggers, stop conditions, and allocation input controls exist. Mark has not independently approved a decision or allocation range; the memo correctly says it is pending. | Mark must select and approve one permitted decision and allocation range, then record that exact approved result in the app and memo. Do not approve Adopt/Allocate while the turnover failure remains unless a documented committee exception is within course scope. |
| 8 | AI disclosure and ownership statements at the beginning of README, memo, and each video script. | **PARTIAL** | [README.md](README.md), [docs/decision-memo.md](docs/decision-memo.md), [docs/transcript-video-1.txt](docs/transcript-video-1.txt), [docs/transcript-video-2.txt](docs/transcript-video-2.txt), [docs/transcript-video-3.txt](docs/transcript-video-3.txt). | Each named file begins with the two required statements. Mark has not yet verified that they accurately reflect his contributions, and [docs/ai-use-log.md](docs/ai-use-log.md) records its entries as pending review/disposition. | Mark should confirm or correct the two opening statements and review each AI-log disposition before final submission. |

## Specific control findings

| Control | Status | Evidence and audit conclusion |
|---|---|---|
| Application startup from README | PASS | Clean virtual environment, documented install, application HTTP response 200. |
| Mandate match | PARTIAL | Core Lab 13 facts are faithfully recorded; full reserve/payout context is not carried into the visible mandate summary. |
| Both baselines and constrained candidate calculated | PARTIAL | Equal weight, risk parity, and candidate calculations run. Candidate math is reproducible, but it does not enforce every hard mandate limit. |
| Accurate mandate verdict for every portfolio | PARTIAL | Rule-by-rule screens are visible and correctly flag the candidate's 35% turnover failure. Liquidity and issuer screens are marked PASS using ETF classification assumptions rather than evidence of fund liquidity/holdings, and thus should be REVIEW for a strict audit. |
| Weights sum to 100% | PASS | Existing test verifies all walk-forward targets; independent constrained check found 100% for both rebalance dates. |
| Optimizer respects all constraints | FAIL | `constrained_low_volatility()` enforces long-only, reserve, min/max, optional volatility cap, and a teaching return proxy, but not annual turnover, full liquidity evidence, actual CPI objective, loss condition, or a specified mandate optimizer target. |
| Point-in-time signal / future leakage | PARTIAL | Date filter and future-data mutation test pass. Lack of time-stamped source availability prevents full proof. |
| Transaction costs | PARTIAL | One-way turnover and cost deductions are implemented consistently and the 25-to-65bp result is reproducible. Turnover is based on target-to-target weights rather than drifted holdings, which limits real-world precision. |
| Stress coverage and stops | PARTIAL | Coverage is present for returns, volatility, correlation, lookback, costs, and experimental constraints. Invalid/infeasible paths stop; stale data and all hard constraints are not fully controlled. |
| Prediction commit / immutability | FAIL | Unverifiable in current environment; locked file has blank hash field. |
| Approved decision, range, monitoring | FAIL | Monitoring exists, but the required Mark-approved decision/range does not. |
| Documentation openings | PARTIAL | Required openings exist; Mark's verification and AI-log review remain outstanding. |
| Reported versus calculated results | PARTIAL | The main constrained metrics and changed-cost mechanics match source data and code. Values contingent on unverified benchmark/global-equity proxy, turnover simplification, liquidity, issuer exemptions, CPI, and loss-rule definitions cannot be fully certified. |

## Prioritized unresolved issues

1. **P0 — No approved final recommendation or allocation range.** This cannot be supplied by the application or AI. Mark must decide after considering the documented evidence.
2. **P0 — The “constrained” candidate violates the 25% turnover cap.** The app correctly stops allocation, but it must either enforce that constraint inside the optimizer or explicitly present no compliant optimized portfolio.
3. **P1 — Changed-input lock sequence cannot be independently proved.** Resolve with the immutable GitHub commit page and a manual content comparison; do not edit the original prediction.
4. **P1 — Benchmark is not a literal global-equity benchmark.** IVV is broad U.S. equity. Use a global-equity series/fund or label results as a benchmark proxy.
5. **P1 — Some PASS verdicts rely on unverified assumptions.** Redeemability and issuer-exemption evidence needs source support or should remain REVIEW.
6. **P1 — The source data cannot establish every point-in-time availability claim.** This is honestly disclosed, but prevents full verification.

## Conclusion

The application is runnable, tested, transparent about several important limitations, and its central displayed constrained-portfolio arithmetic is reproducible from the supplied data. It is **not submission-complete**: the optimizer does not respect all hard mandate constraints, the locked-prediction Git sequence is unverified, and Mark has not approved a final decision/allocation range. No claim of complete compliance should be made until those issues are resolved and the app, documentation, stress outputs, and tests are rerun.
