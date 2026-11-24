# Volatility, ranges and turbulence

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**How large or unstable will the next move be?** Persistence, range information, volatility of volatility and multiscale risk.

In the hourly path study the next-6 h volatility ratio was 0.48 after contraction vs 0.90 in control windows, and 1.50 after expansion vs 0.88. For the BTC 6 h target the earlier path comparison reports R² 0.164 with price features and 0.222 after adding volume shapes. (LOG/PS-01, LOG/PS-03, LOG/PS-11, NB.)

**Test design.** Define forward realised variance as the sum of squared future log returns over a fixed horizon. Record the volatility estimator, units and sampling interval. Include a persistence forecast at the same horizon.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-vol-01"></a>**VOL-01** · From research notes | Multi-horizon volatility or path features improve forward variance forecasts over persistence. | Compare EWMA and short/medium/long realised variance with the same models plus volume shapes and path features, using identical dates, inputs and compute budget. Score 1 h and 6 h forward variance by QLIKE and log-variance error, then leave an asset out. | Minute OHLCV | State M2, Framework FND-008, [LOG/PS-11](#r-log-ps-11). [LOG/PS-01](#r-log-ps-01), [LOG/PS-03](#r-log-ps-03), [LOG/PS-11](#r-log-ps-11), [RV-03](04_participation_returns_and_volatility.md#r-rv-03) |
| <a id="r-vol-02"></a>**VOL-02** · Follow-up | The relative variability of highs and lows predicts asymmetric future excursions after current range and drift are matched. | Estimate variability of changes in log highs and log lows over 30 min. Match total variance, signed momentum and range. Compare next-30-min upward and downward excursions from the decision price. Repeat on quote mids where available. | Minute OHLC and optional quotes | Extension of [LOG/VO-01](#r-log-vo-01). [LOG/VO-01](#r-log-vo-01), [LOG/PR-10](08_jumps_precursors_and_recovery.md#r-log-pr-10) |
| <a id="r-vol-03"></a>**VOL-03** · New question | Two markets with equal current realised variance have different next-hour tail risk if one arrived there through oscillating volatility. | Within the same 60-min variance, recent shock count and final 5-min variance, contrast dispersion of twelve non-overlapping 5-min variance estimates. Score next-hour 95th percentile of absolute return and exceedance calibration. | One-minute returns | New within-window volatility comparison. [LOG/PR-01](08_jumps_precursors_and_recovery.md#r-log-pr-01), [LOG/PR-11](08_jumps_precursors_and_recovery.md#r-log-pr-11) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-p-03"></a>**LOG/P-03** | Shrinking ranges are consolidation and lead to expansion; growing ranges are diffusion (H3) | Efficiency ratio against the mirror surrogate; next range against baseline | "Contraction leads to expansion" failed in 23 of 24 cells. Quiet stays quiet (PS-01). | Unsupported in the source test | PREV, NB |
| <a id="r-log-p-15"></a>**LOG/P-15** | Shrinking ranges have shrinking absolute returns | Range trend over 10 and 30 bars on 1-, 5- and 60-minute bars. Same window (a description), the next half-window, and next volatility at equal current volatility. | Same window: yes, by construction (-0.44 to -0.69 log). Next: the change undoes itself (absolute returns rise 0.08 to 0.30 log in the next half-window) while the level persists (at equal current volatility the next window is 1% to 21% quieter, 6 of 6). | Supported in the source test | HYP |
| <a id="r-log-p-16"></a>**LOG/P-16** | expanding ranges have expanding absolute returns | As P-15, for expanding ranges. | Same window: yes (+0.51 to +0.76 log). Next: absolute returns fall back 0.06 to 0.31 log, and at equal current volatility the next window is 5% to 15% more volatile (6 of 6). | Supported in the source test | HYP |
| <a id="r-log-ps-01"></a>**LOG/PS-01** | After a quiet contraction, volatility stays low | Event vs control bars from the path study, weekly blocks. | Next-6-hour volatility ratio 0.48 against 0.90 (q 0.014). The "plateau" outcome follows 81% of the time against 42%. Still lower after 24 hours (0.68 against 0.98). | Supported in the source test | NB |
| <a id="r-log-ps-02"></a>**LOG/PS-02** | Narrowing ranges with falling volume: volatility stays low | Event vs control bars from the path study, weekly blocks. | 0.75 against 0.91 over 6 hours (q 0.014). Gone after 24 hours. | Supported in the source test | NB |
| <a id="r-log-ps-03"></a>**LOG/PS-03** | After an expansion, volatility stays high | Event vs control bars from the path study, weekly blocks. | 1.50 against 0.88 over 6 hours, 1.54 against 0.95 over 24 hours, 1.63 against 0.89 on 4-hour bars (q 0.014 each). | Supported in the source test | NB |
| <a id="r-log-ps-04"></a>**LOG/PS-04** | Both close gaps narrowing: lower volatility next | Event vs control bars from the path study, weekly blocks. | 0.87 against 0.90 (q 0.18). | Unsupported in the source test | NB |
| <a id="r-log-ps-11"></a>**LOG/PS-11** | Path features forecast the next window's volatility | Event vs control bars from the path study, weekly blocks. | BTC, next 6 hours: R² 0.164 with price only, 0.222 with volume shapes, 0.231 with action, 0.254 with signatures. | Supported in the source test | NB |
| <a id="r-log-vo-01"></a>**LOG/VO-01** | In some periods the highs are the more volatile series (sellers coming down to meet price), in others the lows (buyers coming up) | Open slot. `vt.volatility_features` gives the rolling volatility of O, H, L, C | Not tested. | Untested or no recorded result | No source code supplied |
| <a id="r-log-vo-02"></a>**LOG/VO-02** | Volatility is cyclical: low to high and back | Open slot | The persistence half holds (PS-01, PS-03). Cycle length, and whether the position in the cycle says anything about the next move, are not tested. | Weak or mixed | NB |
| <a id="r-log-vo-03"></a>**LOG/VO-03** | A random forest can call the direction of rolling volatility | Forest against the majority class, a one-line window rule and shuffled bars; 80 / 20 split in time order | No result on file. The earlier cell was interrupted. | Untested or no recorded result | No source code supplied |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-log-idea-19"></a>**LOG-IDEA/19** | High-more-volatile against low-more-volatile periods, and what the highs and lows do next (VO-01). |  | Open in source · Pasted text(20261124-205427).txt §12A |
| <a id="r-log-idea-20"></a>**LOG-IDEA/20** | Length of the volatility cycle, and whether the position in it says anything about the next move (VO-02). |  | Open in source · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/V1, ALPHA/H2, ALPHA/E02, ALPHA/E13, ALPHA/E23

