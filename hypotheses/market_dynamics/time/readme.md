| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| T-01 | Asset longevity has an impact (however minor) on price stability | Monthly realised vol vs log(asset age), compared within each month; asset-clustered SE; Fama–MacBeth Spearman check | β = −0.229 (SE 0.103, one-sided p = 0.013): doubling an asset's age ≈ −14.7% vol. Fama–MacBeth mean ρ(age, vol) = −0.46 (NW t = −14.59, 71 months, 7 assets). | Proved | notebook test |
| T-02 | There are specific hour+day combinations with higher/lower average absolute returns than others. Could this be the weekends or middle of the night? | 168-cell hour-of-week permutation test (within-week circular shifts) on normalised \|returns\|; weekend and night ratio tests | Significant in 7/7. Weekend/weekday \|ret\| 0.71× (7/7); night (00–06 UTC)/rest 0.93× (7/7). Highest: Tue 14h 1.67×, Thu 15h 1.57×, Fri 14h 1.57×. Lowest: Sun 06h 0.64×, Sun 05h 0.65×, Sat 19h 0.66×. | Proved | notebook test |
| T-03 | Does the US Open have a higher volume than the rest of the hours on average? | 09:30–10:30 ET dollar volume vs the other 23 hours on NYSE trading days (Wilcoxon); DST check | Open hour 1.57× the other hours, significant in 7/7; highest hour of the day (median rank 1/24, peak 09:30 ET). The peak follows the NY clock through DST in 7/7. | Proved | notebook test |
| T-04 | Is volume seasonal to the hour of the day, day of week, day of month? | Permutation tests on log dollar volume: hour of day, day of week, day of month | Hour of day 7/7 (peak-to-trough 1.98×); day of week 7/7 (1.60×); day of month 1/7 (1.29×, not significant). | Partially proved (hour & weekday yes, day of month no) | notebook test |
| T-05 | Are prices seasonal to the hour of the day, day of week, day of month? | ADF unit-root test on log price; STL seasonal strength of the price level vs shuffled surrogates; day-of-month level test | Price-level seasonality in 0/7 for every component. Seasonal amplitude (hour 78.2 bps, weekday 101.1 bps) is no bigger than reshuffled data produces; median daily move is 193 bps. Unit root not rejected in 4/7. | Disproved | notebook test |
| T-06 | Is returns seasonal to the hour of the day, day of week, day of month? | Permutation tests on vol-scaled signed returns: hour of day, day of week, day of month | Hour of day 1/7, day of week 0/7, day of month 0/7. The direction of returns is not seasonal; only their size is (T-02). | Disproved | notebook test |
| T-07 | Is there a semi stable cyclical order between major bearish and bullish periods? | ±30% bull/bear dating; spread of cycle lengths (CV) vs bootstrapped random walks | Enough cycles in 7/7. Median cycle 170 days, but cycle lengths are *less* regular than random walks (CV 0.82 vs 0.61): no stable rhythm. | Disproved | notebook test |
| T-08 | Can we detect mean and confidence intervals for the time distance between major drops/jumps? | Moves above 5σ (trailing vol), at most one per 24h; bootstrap CI of mean gap; Weibull shape; KS test vs exponential | Detectable in 7/7: median mean gap 17.3 days (CI ≈ 14.4–20.6). Gaps are close to exponential (Weibull k = 1.02): the rate can be estimated, but the time since the last event doesn't predict the next. | Proved (rate yes, timing memoryless) | notebook test |
| T-09 | Can we show that there is a high likelihood that a jump/drop doesn't occur on a saturday/sunday? | Weekend share of big moves vs 2/7 (binomial), raw-size and vol-scaled definitions, pooled on unique event days | Raw-size: 18.1% of 127 event days fall on weekends vs 28.6% expected (pooled p = 0.0046); single coins have too few events to show it (0/7). Vol-scaled: 15.9% (p = 2.7e-9), lower in 7/7. Weekends have fewer shocks even after allowing for their lower volatility. | Proved (pooled) | notebook test |
| T-10 | The markets should be event clocked rather than time clocked. | Same 1-min path sampled by time, volume, dollar and trade-count clocks with equal bar counts; kurtosis, Moors kurtosis, vol clustering | *Earlier run (4 event-clock assets):* dollar clock beats time clock in 3/4. Excess kurtosis 37.9 → 20.9; Moors 1.55 → 1.36 (normal 1.23); vol clustering 2.53 → 3.43 (worse). | Proved (on tails; clustering not reduced) | notebook test (earlier run) |
| T-11 | The event clock should be fractal, rather than discretionary | Multifractal partition function of dollar volume (1 min–11 days) vs shuffled data; dollar-bar variance vs bar size | *Earlier run (4 event-clock assets):* scale-free in 4/4 (variance ∝ size^0.96, H ≈ 0.48, no privileged bar size); not multifractal (Δα 0.22 < shuffled 0.29). | Partially proved (scale-free, monofractal) | notebook test (earlier run) |

Run: 24 Nov 2026 · `C:\marketdata\crypto` (Binance, 1-min) · 7 assets. "Proved" = significant at 5% in ≥ 2/3 of assets after BH correction across assets, unless the row says otherwise.

# Calendar, sessions and asset age

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**When does activity differ?** Age, seasonality, sessions and day-of-week effects.

On the 7-asset run, opening-hour volume is 1.57× the other hours and the spike shrinks on NYSE holidays (CAL-02). Weekend absolute returns are 0.71× weekday, while the night effect on returns is small (0.93×). Volume varies by hour and weekday but not by day of month, including all named calendar days (T-04b). Older coins are calmer: the effect survives a dollar-volume control, but it comes from comparing coins with each other, not from a coin calming down as it ages (T-01b). (T-01–T-06, CAL-01, CAL-02, VT-05.)

**Test design.** Use minutes actually observed. Record UTC and the relevant exchange calendar. For seasonal baselines use comparable past weekdays and handle holidays and daylight-saving changes. Listing age and calendar date need separate identification.

### Your current rows

| ID | Hypothesis | Test or next comparison | Reported result and scope | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-01"></a>**T-01** | Older assets have more stable prices, measured by lower volatility. | Log monthly volatility vs log age with month fixed effects (asset-clustered SE); Fama–MacBeth Spearman check. Follow-up in T-01b / CAL-01. | β = −0.229 (SE 0.103, one-sided p = 0.013). Doubling age ≈ 14.7% lower volatility. Fama–MacBeth mean rank correlation −0.46, Newey–West t = −14.59, 71 months, 7 assets. | Supported in sample · "Proved" | Notebook run 24 Nov 2026 |
| <a id="r-t-01b"></a>**T-01b** | The age effect survives a liquidity/size control and leave-one-asset-out. | Adds log dollar volume to the T-01 regression; drops each coin in turn; within-coin slope (asset FE + market vol); age-band profile. | With dollar volume controlled, β_age = −0.206 (p = 0.013) and dollar volume is not significant (β = −0.033, p = 0.19). β negative in 100% of leave-one-out fits (−0.27 to −0.14). Within-coin slope −0.091, not significant (p = 0.385). 5/6 age bands have ≥ 2 coins. | Supported in sample · "Proved" (cross-sectional; within-coin not shown) | Notebook run 24 Nov 2026 |
| <a id="r-t-02"></a>**T-02** | Absolute returns vary across hour-of-week cells, including weekends and 00–06 UTC. | Each asset's mean absolute return across weekday × hour cells (circular-shift permutation test), plus prespecified weekend and night contrasts. | Significant in 7/7. Weekend/weekday ratio 0.71 and night/rest ratio 0.93, both lower in 7/7. Highest Tue 14h 1.67×, Thu 15h 1.57×, Fri 14h 1.57×. Lowest Sun 06h 0.64×, Sun 05h 0.65×, Sat 19h 0.66×. | Supported in sample · "Proved" (night effect small) | Notebook run 24 Nov 2026 |
| <a id="r-t-03"></a>**T-03** | Crypto volume is higher during the US equity opening hour than in other hours. | 09:30–10:30 ET vs the other 23 hours on NYSE trading days; DST difference-in-differences. | Opening-hour volume 1.57× the other hours at the median, significant in 7/7. Peak follows the NY clock through DST in 7/7. Median opening-hour rank 1/24; most common peak 09:30 ET. | Supported in sample · "Proved" | Notebook run 24 Nov 2026 |
| <a id="r-t-04"></a>**T-04** | Volume has hour-of-day, day-of-week and day-of-month patterns. | Circular-shift permutation test for each component per asset; peak-to-trough ratio. Day-of-month follow-up in T-04b. | Hour significant in 7/7, 1.98× peak to trough. Weekday significant in 7/7, 1.60×. Day of month significant in 1/7, 1.29×. | Partial support · "Partially proved" | Notebook run 24 Nov 2026 |
| <a id="r-t-04b"></a>**T-04b** | A specific day-of-month volume effect exists. | Trend- and weekday-adjusted daily log volume; named contrasts with Newey–West t, BH. | Turn of month −4.9% (0/7), month end −4.1% (0/7), expiry Friday −2.7% (0/7), quarterly expiry +9.4% (0/7), first half +3.7% (0/7). | No support in sample · "Disproved": no day-of-month volume effect | Notebook run 24 Nov 2026 |
| <a id="r-t-05"></a>**T-05** | Price levels exhibit a stable hour-of-day, weekday or day-of-month pattern. | ADF unit-root test; STL seasonal strength vs within-block-rotated surrogates; day-of-month level test. | Unit root not rejected in 4/7. Seasonal level components in 0/7 for each frequency. Median hour-of-day/day-of-week amplitudes 78.2/101.1 bp against a 193 bp median daily move. | No support in sample · "Disproved" | Notebook run 24 Nov 2026 |
| <a id="r-t-06"></a>**T-06** | Mean signed returns vary predictably by hour, weekday or day of month. | Permutation tests on vol-scaled signed returns. Held-out check in T-06b. | Significant in 1/7 for hour, 0/7 for weekday, 0/7 for day of month. T-02 shows a seasonal pattern in absolute returns only. | No support in sample · "Disproved" | Notebook run 24 Nov 2026 |
| <a id="r-t-06b"></a>**T-06b** | Return-seasonality cells chosen in the first half persist in the second half and beat costs. | Top 3 hours + top weekday picked on the first half (pooled), validated per asset on the second half; 18 bps cost bar. | Tue (+) −45.1 bps OOS, 0/7; hour 10 (+) −1.4 bps, 0/7; hour 14 (−) +1.6 bps, 0/7; hour 22 (+) +3.2 bps, 1/7. None validates or clears 18 bps. | No support in sample · "Disproved": no tradeable return seasonality | Notebook run 24 Nov 2026 |
| <a id="r-vt-01"></a>**VT-01** | Volume has within-day and within-week seasonality. | Hour and weekday components of T-04, with day of month kept separate. | Hour significant in 7/7 (1.98×), weekday in 7/7 (1.60×). | Same claim as T-04 components · "Proved" | Notebook run 24 Nov 2026 via T-04 |
| <a id="r-vt-02"></a>**VT-02** | Volume increases during the US opening hour. | T-03 New York opening-hour comparison. | Opening-hour volume 1.57× other hours, 7/7. | Same claim as T-03 · "Proved" | Notebook run 24 Nov 2026 via T-03 |
| <a id="r-vt-03"></a>**VT-03** | Crypto volume decreases around the US equity close. | log(16:00–17:00 ET / 15:00–16:00 ET volume) on NYSE days vs weekends and NYSE holidays (placebo). | Not in the supplied output; copy from the notebook's VT-03 row. | Tested · result to be entered | Notebook run 24 Nov 2026 |
| <a id="r-vt-04"></a>**VT-04** | Weekend crypto volume is lower than weekday volume. | Weekly weekend/weekday ratio at matching UTC hours, bootstrap CI over weeks, split by vol regime. | Not in the supplied output; copy from the notebook's VT-04 row. T-04's weekday effect (1.60×) points the same way. | Tested · result to be entered | Notebook run 24 Nov 2026 |
| <a id="r-vt-05"></a>**VT-05** | Volume is lower at night. | Weekday per-day log(mean volume 00–06 / rest), Wilcoxon; night defined in UTC (primary), New York and Singapore time. | UTC night 0.80× rest (7/7); NY night 0.84× (7/7); Singapore night 0.94× (7/7). The UTC and NY nights are the quietest; Asia's night is only slightly quieter. | Supported in sample · "Proved" | Notebook run 24 Nov 2026 |

### Proposed tests

Both rows have now been run.

| ID and origin | Hypothesis | Decisive comparison | Result | Assessment | Source and related IDs |
| --- | --- | --- | --- | --- | --- |
| <a id="r-cal-01"></a>**CAL-01** · Follow-up | Volatility falls as a coin ages even within comparable listing cohorts and market conditions. | Age-band volatility with month fixed effects (common support reported); within-coin slope with market-vol control; dollar-volume (liquidity) control. Delisted coins are not in the data. | Same run as T-01b: age effect survives the liquidity control (β = −0.206, p = 0.013) and every leave-one-out fit. Within-coin slope −0.091 is not significant (p = 0.385). 5/6 age bands have ≥ 2 coins. | Supported cross-sectionally · within-coin aging not established | Notebook run 24 Nov 2026. [T-01](#r-t-01), [T-01b](#r-t-01b) |
| <a id="r-cal-02"></a>**CAL-02** · Follow-up | The 09:30 ET volume spike weakens when the US cash-equity market is closed. | log(09:30–10:30 ET / 08:30–09:30 ET) volume on NYSE trading days vs weekday NYSE holidays (Mann–Whitney); weekend placebo; DST-mismatch weeks. | Spike 1.31× on trading days vs 1.09× on NYSE holidays (≈56 holidays) and 1.05× at weekends; weaker on holidays in 5/7. Open/next-hour ratio 1.18× in DST-mismatch weeks vs 1.17× normally, so the spike stays on the NY clock. | Supported in sample · "Proved" | Notebook run 24 Nov 2026. [T-03](#r-t-03), [VT-02](#r-vt-02), [VT-03](#r-vt-03) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide. The last two columns show this notebook's re-run.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code | Re-run 24 Nov 2026 | Re-run status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <a id="r-log-v-16"></a>**LOG/V-16** | Volume is seasonal within the day and the week | `volume_signals`, question 1 | Your note: yes, by hour and by weekday. | Supported in the source test | NOTE | Via T-04: hour 7/7 (1.98×), weekday 7/7 (1.60×). | Supported |
| <a id="r-log-v-17"></a>**LOG/V-17** | The volume level was properly adjusted before the tests (weekday, daylight saving) | Old against new relative volume | Your note: "already accounted for". | Supported in the source test | NOTE | Relative volume vs trailing 8-week same-hour mean. Residual hour-of-week seasonality: 1/7 on both the UTC and NY clocks. Open-hour volume in the 3 weeks after each DST switch is +23.9% (UTC clock) / +19.2% (NY clock) above normal, significant in 7/7 on both. | Not supported: adjustment incomplete around DST switches |
| <a id="r-log-p-09"></a>**LOG/P-09** | Today opens below yesterday's close and low: the high ends above the open, or above yesterday's low (H8) | New York session gaps against the mirror | Null so far. | Unsupported in the source test | PREV | After a gap down (median 369 days per asset) the session closes above its open 50.3% vs 48.5% otherwise; significant in 1/7. Gap-fill to prior low 42.6% vs mirror 45.4%. | Not supported (null confirmed) |
| <a id="r-log-p-10"></a>**LOG/P-10** | The direction of the US open, from 5 minutes before to 15 minutes after, continues (H9) | Continuation and pullback entries | Null so far. | Unsupported in the source test | PREV | Continuation hit rate 47.7% (→10:30) and 48.7% (→16:00), 0/7 significant. Mean signed return −1.4 / +4.4 bps (large signals −3.6 bps) vs 18 bps cost. | Not supported (null confirmed) |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-log-idea-18"></a>**LOG-IDEA/18** | Session-gap and US-open tests on index data (the optional US500 and US100 download was not run). | P-09 / P-10 tests run on index data once `INDEX_FILES` is set in the notebook. | Open: index data not loaded · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-31, ALPHA/V3

# Regimes, cycles and big moves

**Is there structure in when big moves and regimes happen?** Bull/bear cycles, gaps between major moves, and weekends.

There is no regular bull/bear rhythm. Big moves arrive about every 17 days at random with respect to elapsed time. Volume surges and the weekend are strongly linked to them, but the out-of-sample AUC of 0.60 makes the timing only weakly predictable. Weekends see fewer big moves than expected, beyond their lower volatility. (T-07–T-09, T-08b.)

| ID | Hypothesis | Test | Result | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-07"></a>**T-07** | There is a semi-stable cyclical order between major bearish and bullish periods. | ±30% bull/bear dating; CV of cycle lengths vs stationary-block-bootstrap random walks. | 7/7 resolvable; median cycle 170 days; CV 0.82 vs null 0.61; more regular than random in 0/7. | No support in sample · "Disproved" (cycles less regular than random) | Notebook run 24 Nov 2026 |
| <a id="r-t-08"></a>**T-08** | The mean and CI of the time between major drops/jumps can be detected. | Moves above 5σ (trailing vol), at most one per 24h; bootstrap CI of mean gap; Weibull shape; KS vs exponential. | Detectable in 7/7; median mean gap 17.3 days (CI ≈ 14.4–20.6); Weibull k = 1.02 (memoryless). | Supported in sample · "Proved" (rate yes, elapsed-time timing no) | Notebook run 24 Nov 2026 |
| <a id="r-t-08b"></a>**T-08b** | The timing of major moves is predictable from market state. | Pooled logistic for an event in the next 24h ~ trailing-vol percentile, hours since last event, weekend, volume surge, asset FE; out-of-sample AUC on the last 40%. | AUC 0.600 (base rate 6.0%). Odds ratios: volume surge 2.49 (p ≈ 0); weekend 0.66 (p = 4.5e-64); vol percentile 0.59 (p = 5.6e-65, lower because events are defined relative to trailing vol); hours since last event 1.02 (p = 0.013). | Borderline · notebook "Disproved" at the AUC ≥ 0.60 cut; volume surge strongly predictive | Notebook run 24 Nov 2026 |
| <a id="r-t-09"></a>**T-09** | A jump/drop is unlikely to occur on a Saturday/Sunday. | Weekend share of events vs 2/7 (binomial), raw-size and vol-scaled definitions, pooled on unique event days. | Raw-size: 18.1% of 127 event days (p = 0.0046); per asset 0/7 (too few events). Vol-scaled: 15.9% (p = 2.7e-9), lower in 7/7. | Supported in sample · "Proved (pooled)" | Notebook run 24 Nov 2026 |

# Market clocks, scaling and transfer

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**Does sampling change what we learn?** Time bars, dollar bars, scale laws and transfer to unseen assets.

In the earlier run, equal-dollar bars cut excess kurtosis from 37.9 to 20.9 and moved Moors kurtosis toward normal (1.55 → 1.36), but volatility clustering rose (2.53 → 3.43). Scaling is clean (H ≈ 0.48) without multifractality. On the 7-asset run, dollar bars add almost nothing to volatility forecasts (CLK-01) or to direction prediction (N-16). Volatility forecasts transfer across coins once features are put in vol units (N-19). In standard units the coins' return distributions roughly line up, but the hour of day does not drop out (ST-01). (T-10, T-11, CLK-01, LOG/ST-01, DRAFT/N-16, DRAFT/N-19.)

**Test design.** Use actual trade notional where available. A clock made from whole minute bars is an approximation. Record duration and overshoot. Compare at common decision times and on identical future clock-time labels.

### Your current rows

| ID | Hypothesis | Test or next comparison | Reported result and scope | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-10"></a>**T-10** | Equal-dollar bars produce more stable return statistics than equal-time bars. | Time, volume, dollar and trade-count clocks on the same 1-min path with equal bar counts; excess kurtosis, Moors kurtosis, vol clustering. Robustness in T-10b. | Dollar clock beat time clock on ≥ 2/3 metrics in 3/4 assets. Median excess kurtosis 37.9 → 20.9, Moors kurtosis 1.55 → 1.36 (normal reference 1.23), volatility clustering 2.53 → 3.43. | Mixed by metric · "Proved" on tails, clustering worse | Earlier notebook run (4 event-clock assets); T-10b not in supplied output |
| <a id="r-t-11"></a>**T-11** | Event-time price variation scales consistently across bar sizes and shows multifractality. | Multifractal partition function of dollar volume (1 min–11 days) vs shuffled; dollar-bar variance across bar sizes. | Scaling R² ≥ 0.98 in 4/4; Δα above shuffled in 0/4 (median 0.222 vs 0.291 shuffled). Dollar-bar variance ∝ size^0.96 (R² 1.000), H ≈ 0.48. | Scale-free, not multifractal · "Partially proved" | Earlier notebook run (4 event-clock assets) |

### Proposed tests

| ID and origin | Hypothesis | Decisive comparison | Result | Assessment | Source and related IDs |
| --- | --- | --- | --- | --- | --- |
| <a id="r-clk-01"></a>**CLK-01** · Follow-up | Dollar bars improve forecasts of the same next hour's realised variance beyond the trading-rate information available in minute bars. | Next-60-min log realised variance at identical hourly timestamps. HAR + dollar-volume time model vs the same + adaptive dollar-bar features (whole-minute approximation). Trained on the first 60%, tested on the last 40%, Diebold–Mariano. | Out-of-sample R²: time model 0.728, + dollar-bar features 0.729, dollar bars alone 0.694. MSE improvement 0.5%, significant in 4/7. | Mixed · statistically detectable in some coins, economically negligible | Notebook run 24 Nov 2026. [T-10](#r-t-10), [DRAFT/N-16](#r-draft-n-16) |
| <a id="r-clk-02"></a>**CLK-02** · Follow-up | The excess multifractal width survives heavy tails, finite samples and changes of volatility regime. | MF-DFA (q −4…4) on dollar-bar returns; Δα vs shuffled and IAAFT surrogates; repeated on ~yearly blocks. | Not in the supplied output; copy from the notebook's CLK-02 row. | Tested · result to be entered | Notebook run 24 Nov 2026. [T-11](#r-t-11) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide. The last two columns show this notebook's re-run.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code | Re-run 24 Nov 2026 | Re-run status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| <a id="r-log-st-01"></a>**LOG/ST-01** | In standard units the coins have the same distribution, and the hour of day drops out | Quantiles of raw and standardised returns, volume and ranges per coin; spread of each quantile across the six coins relative to its size; hour-of-day profile. | Yes for returns, mostly for volume. The 1% and 99% points of z differ across coins by 2% to 3% of their size (raw returns: 33% to 35%), the 0.1% points by 7% to 10% (raw: 34% to 36%). Relative volume agrees within 2% to 9% above the median; below it the thin coins still differ by 23% to 31% (more near-empty minutes). BTC by hour: the raw absolute return swings from 0.80 to 1.38 of its daily mean and raw volume from 0.75 to 1.56; z2 stays within 0.97 to 1.02 and relative volume within 0.96 to 1.05. | Supported in the source test | HYP | Out of sample (hour factor from first half, tested on second half). 1%/99% spread across coins: raw 50%/52% → z 9%/10%; 0.1%/99.9%: z 16%/23%. Relative volume spread 7% (75th pct) / 11% (25th pct). z² hour profile 0.68–1.45 of its mean; hour effect left in 5/7. | Partly supported: distributions align; hour of day does not drop out out-of-sample (notebook "Disproved") |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Re-run 24 Nov 2026 | Status / provenance |
| --- | --- | --- | --- | --- |
| <a id="r-draft-n-16"></a>**DRAFT/N-16** | A fixed pattern of price/volume behaviour predicts the next hourly outcome more consistently in equal-notional-volume bars than in equal-clock-time bars. Shortening bar completion times may add further information. | Construction — Set each asset's notional threshold from prior complete days; for example, the prior 20-day median daily quote notional divided by 288. Freeze it for the next day. The numerical target is a starting choice, not an optimum. Minute-only approximation — Aggregate whole completed minute bars until the threshold is crossed. Keep overshoot and elapsed time. Do not split or reorder unknown intraminute trades. Exact volume bars require trade-level data. Features — Apply a prespecified price-pattern feature to the completed volume bars; include bar duration relative to normal duration for that time of week. Outcome — The same next 60/240 clock minutes used by the time-bar model. Comparing next five volume bars with next five minutes would confound the horizon. Null — Time-bar features with equivalent elapsed-history coverage and controls for recent volume/volatility; compare at the same real decision timestamps. | 3-bar sign pattern (+ relative bar duration) vs 5-min bars, same controls and hourly decision times, logistic, out of sample. Log-loss gain vs constant model: time bars −0.01% (60 min) / −0.04% (240 min); dollar bars −0.02% / −0.04%. Dollar model better than both time model and constant in 0/7. | Tested · not supported: the pattern has no signal in either clock · Pasted text(20261124-205427).txt §8 |
| <a id="r-draft-n-19"></a>**DRAFT/N-19** | The same prespecified mapping from normalised features to outcomes works on assets absent from model fitting, in a later test period. | Test — Compare volatility units, robust seasonal z-scores, and trailing percentile ranks using the same model, feature set and temporal splits. Hold out complete assets AND later dates. The held-out asset's own past may update an unsupervised scaler; its outcomes cannot be used to tune the mapping. Outcomes — Incremental predictive value, calibration, and effect size across held-out assets and regimes. Report market-factor and asset-specific results separately. Null — Per-asset models, raw-feature pooled models, and M0. Similar-looking charts without held-out outcome improvement do not pass. | Leave-one-asset-out, later 40% of dates. Volatility target: vol-unit features, OOS R² 0.616 (positive 7/7; beats raw pooled 0.303 in 7/7; per-asset model 0.620). Direction target: best seasonal z, OOS R² 0.0001 (positive 4/7). | Tested · supported for volatility, not for direction · Pasted text(20261124-205427).txt §8 |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/IND-16, ALPHA/E24, ALPHA/SC-01

## Add a hypothesis

| ID | Hypothesis | Test / benchmark | Result | Status | Source |
| --- | --- | --- | --- | --- | --- |
| New unique ID | One claim that could fail | Observable trigger at t, target horizon, comparison group, effect measure |  | Proposed | Author, parent ID or source row |

When recording a run, add the date, dataset and universe, independent dates/events, effect and interval, and any competing outcome. For a revised definition, give the variant a suffix and retain the earlier result.
