
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| T-01 | . | . | β = -0.127 (SE 0.152, one-sided p = 0.202); doubling an asset's age changes its vol by -8.4%. Fama–MacBeth mean ρ(age, vol) = -0.23 (NW t = -9.42, 71 months, 9 assets). | Unresloved | notebook test |
| T-02 | . | . | Significant in 9/9 assets. Weekend/weekday | Proved | notebook tests |
| T-03 | . | . | Open-hour volume is 1.57× the other hours (median), significant in 9/9 assets; median rank of the open hour 1/24 (most common peak 09:30 ET). Volume follows the NY clock through DST in 9/9 assets. | Proved | notebook tests |
| T-04 | . | . | HOD: significant in 9/9 (Proved), peak-to-trough 1.97×; DOW: significant in 9/9 (Proved), peak-to-trough 1.58×; DOM: significant in 2/9 (Disproved), peak-to-trough 1.33× | partially proved | notebook test |
| T-05 | . | . | Unit root not rejected in 5/9 assets (levels are non-stationary). Price-level seasonality significant in any component for 0/9 assets (HOD 0, DOW 0, DOM 0). Median seasonal amplitude HOD 78.2 bps, DOW 98.9 bps vs a median daily move of 193 bps. | Disproved | notebook test |
| T-06 | . | . | HOD: significant in 3/9 (Disproved); DOW: significant in 1/9 (Disproved); DOM: significant in 0/9 (Disproved). Signed-return seasonality is much weaker than volatility seasonality (T-02); check whether the effect survives costs. | Disproved| notebook test |
| T-07 | . | . | 9/9 assets resolvable; cycle lengths more regular than bootstrap random walks in 0/9. Median cycle 170 days, CV 0.84 vs null 0.61. | Disproved | notebook test |
| T-08 | . | . | Mean gap detectable (≥30 events, CI ±≤25%) in 9/9 assets; median mean-gap 17.3 days (typical CI 14.3–20.4). Weibull k < 1 (clustered, not memoryless) in 0/9 assets (median k = 1.03). Gaps look close to exponential (memoryless): the rate is estimable, but the timing of the next event is not predictable from the last one. | Proved | notebook test |
| T-09 | . | . | Raw-size events: weekend share 19.0% (expected 28.6%), significantly lower in 0/9 assets; pooled 19.6% of 153 event days (p = 0.0074). Vol-scaled events: 16.7%, lower in 9/9; pooled 17.4% (p = 4e-09). If only the raw share is low, weekend calm comes from lower volatility, not from weekends suppressing shocks. | Proved | notebook test |
| T-10 | . | . | Dollar clock beats time clock (≥2/3 metrics) in 3/4 assets. Median excess kurtosis 37.9 → 20.9; Moors 1.55 → 1.36 (normal 1.23); vol clustering 2.53 → 3.43. | Proved | notebook test |
| T-11 | . | . | Scale-invariant (R² ≥ 0.98) in 4/4 and multifractal (Δα > shuffled) in 0/4; both in 0/4. Median Δα = 0.222 vs shuffled 0.291. Dollar-bar variance ∝ size^0.96 (R² 1.000), i.e. H ≈ 0.48 in event time, with no privileged bar size. | Disproved | notebook test |



# Calendar, sessions and asset age

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**When does activity differ?** Age, seasonality, sessions and day-of-week effects.

Your six-asset notebook gives an opening-hour volume ratio of 2.86× and a weekend/weekday absolute-return ratio of 0.76. The weekday-volume result establishes differences across days. A specific weekend volume ratio has its own entry. (T-02–T-04, VT-04.)

**Test design.** Use minutes actually observed. Record UTC and the relevant exchange calendar. For seasonal baselines use comparable past weekdays and handle holidays and daylight-saving changes. Listing age and calendar date need separate identification.

### Your current rows

| ID | Hypothesis | Test or next comparison | Reported result and scope | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-01"></a>**T-01** | Older assets have more stable prices, measured by lower volatility. | Compare log volatility with log listing age. Retain the reported cross-sectional estimates and test listing cohorts at comparable ages and market conditions. | β = −0.379 (SE 0.040, one-sided p = 8.31×10⁻²²). Doubling age corresponds to 23.1% lower volatility. Fama–MacBeth mean rank correlation −0.81, Newey–West t = −33.10, 24 months and six assets. | Supported in sample · supplied “proved” | Current notebook summary |
| <a id="r-t-02"></a>**T-02** | Absolute returns vary across hour-of-week cells, including weekends and 00–06 UTC. | Compare each asset’s mean absolute return across weekday × hour cells and the prespecified weekend and night contrasts. | Significant in 6/6 assets. Median weekend/weekday and night/rest ratios both 0.76, lower in all six. Highest Wed 14h 1.44×, Mon 14h 1.40×, Fri 14h 1.37×. Lowest Sun 03h 0.66×, Sun 04h and Sat 04h 0.68×. | Supported in sample · supplied “proved” | Current notebook summary |
| <a id="r-t-03"></a>**T-03** | Crypto volume is higher during the US equity opening hour than in other hours. | Anchor the hour to 09:30 New York time. Compare other hours, including the UTC hour displaced by the US daylight-saving change. | Opening-hour volume 2.86× the other hours at the median. Significant and NY-clock aligned in 6/6 assets. Median opening-hour rank 1/24. Most common peak 09:30 ET. | Supported in sample · supplied “proved” | Current notebook summary |
| <a id="r-t-04"></a>**T-04** | Volume has hour-of-day, day-of-week and day-of-month patterns. | Estimate each component per asset. Compare its out-of-period profile and peak-to-trough ratio. | Hour significant in 6/6, 2.81× peak to trough. Weekday significant in 6/6, 1.72×. Day of month significant in 0/6, 1.27×. | Partial support · supplied “partially proved” | Current notebook summary |
| <a id="r-t-05"></a>**T-05** | Price levels exhibit a stable hour-of-day, weekday or day-of-month pattern. | Retain the unit-root and seasonal-component tests. A follow-up can test detrended levels against random-walk placebos. | Unit root not rejected in any of six series. Seasonal level components detected in 0/6 for each frequency. Median hour-of-day/day-of-week amplitudes 66.4/121.5 bp against a 265 bp median daily move. | No support in sample · supplied “Disproved” | Current notebook summary |
| <a id="r-t-06"></a>**T-06** | Mean signed returns vary predictably by hour, weekday or day of month. | Estimate signed-return effects separately from absolute moves. Check predeclared cells on held-out dates. | Significant in 0/6 for hour, 0/6 for weekday and 0/6 for day of month. T-02 shows a seasonal pattern in absolute returns. | No support in sample · supplied “Disproved” | Current notebook summary |
| <a id="r-vt-01"></a>**VT-01** | Volume has within-day and within-week seasonality. | Use the hour and weekday components of T-04. Keep day of month as a separate component. | T-04 reports significance for both components in 6/6 assets. Earlier notes also report both patterns. | Same claim as T-04 components | Current list + T-04, [LOG/V-16](#r-log-v-16) (NOTE) |
| <a id="r-vt-02"></a>**VT-02** | Volume increases during the US opening hour. | Use the T-03 New York opening-hour comparison. | Opening-hour volume 2.86× other hours at the median in T-03. | Same claim as T-03 | Current list + T-03 |
| <a id="r-vt-03"></a>**VT-03** | Crypto volume decreases around the US equity close. | Compare a frozen New York close window (e.g. final 30 min before 16:00 ET) with preceding and following eligible half-hours and the same time on US holidays. Report each contrast. | Open. The opening-hour result does not establish a closing-hour decrease. | Open | Current list |
| <a id="r-vt-04"></a>**VT-04** | Weekend crypto volume is lower than weekday volume. | Compare observed weekend and weekday volume at matching UTC hours and asset. Estimate the ratio and interval by week and market regime. | T-04 shows weekday variation. The supplied summary gives no direct weekend/weekday volume ratio. The 0.76 in T-02 concerns absolute returns. | Open for this contrast | Current list + T-02, T-04 |
| <a id="r-vt-05"></a>**VT-05** | Volume is lower at night. | Fix night as 00–06 UTC to align with T-02. Compare matched weekdays and report how conclusions change with the chosen time zone. | T-04 establishes hourly variation. The supplied night/rest ratio of 0.76 in T-02 measures absolute returns. | Open for this contrast | Current list + T-02, T-04 |

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-cal-01"></a>**CAL-01** · Follow-up | Volatility falls as a coin ages even within comparable listing cohorts and market conditions. | Compare age–volatility slopes within staggered cohorts at overlapping ages. Match lagged liquidity and broad market volatility, include delisted names where available and report common support. Compare within-coin and cross-sectional slopes. Age equals calendar date minus listing date. State which age, period or cohort effects are constrained before estimating a slope. | Listing and delisting dates, daily returns, venue liquidity | Proposed from T-01. [T-01](#r-t-01) |
| <a id="r-cal-02"></a>**CAL-02** · Follow-up | The 09:30 ET volume spike weakens when the US cash-equity market is closed. | Compare 09:30–10:30 ET minus a neighbouring pre-open hour on full trading days and weekday holidays. Match weekday, month and BTC volatility. Use a displaced-hour placebo during DST mismatch weeks. | Crypto quote notional and US exchange calendar | Proposed from T-03 and VT-03. [T-03](#r-t-03), [VT-02](#r-vt-02), [VT-03](#r-vt-03) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-v-16"></a>**LOG/V-16** | Volume is seasonal within the day and the week | `volume_signals`, question 1 | Your note: yes, by hour and by weekday. | Supported in the source test | NOTE |
| <a id="r-log-v-17"></a>**LOG/V-17** | The volume level was properly adjusted before the tests (weekday, daylight saving) | Old against new relative volume | Your note: "already accounted for". | Supported in the source test | NOTE |
| <a id="r-log-p-09"></a>**LOG/P-09** | Today opens below yesterday's close and low: the high ends above the open, or above yesterday's low (H8) | New York session gaps against the mirror | Null so far. | Unsupported in the source test | PREV |
| <a id="r-log-p-10"></a>**LOG/P-10** | The direction of the US open, from 5 minutes before to 15 minutes after, continues (H9) | Continuation and pullback entries | Null so far. | Unsupported in the source test | PREV |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-log-idea-18"></a>**LOG-IDEA/18** | Session-gap and US-open tests on index data (the optional US500 and US100 download was not run). |  | Open in source · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-31, ALPHA/V3

# Market clocks, scaling and transfer

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**Does sampling change what we learn?** Time bars, dollar bars, scale laws and transfer to unseen assets.

Equal-dollar bars reduced the reported volatility clustering and moved Moors kurtosis toward its reference value. Median excess kurtosis was essentially unchanged. Similar return distributions in standard units were also reported across six coins. (T-10, T-11, LOG/ST-01.)

**Test design.** Use actual trade notional where available. A clock made from whole minute bars is an approximation. Record duration and overshoot. Compare at common decision times and on identical future clock-time labels.

### Your current rows

| ID | Hypothesis | Test or next comparison | Reported result and scope | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-10"></a>**T-10** | Equal-dollar bars produce more stable return statistics than equal-time bars. | Compare the same three diagnostics at comparable sample sizes. Record bar construction and elapsed duration. | Dollar bars won on at least 2/3 metrics in 2/3 assets. Median excess kurtosis 62.4 → 62.5, Moors kurtosis 1.32 → 1.26 (normal reference 1.23), volatility clustering 0.08 → approximately zero. | Mixed by metric and asset · supplied “Proved” | Current notebook summary |
| <a id="r-t-11"></a>**T-11** | Event-time price variation scales consistently across bar sizes and shows multifractality. | Estimate the scaling slope across prespecified sizes and compare spectral width with matched surrogate series. | Scaling R² ≥0.98 and Δα above shuffled values in 3/3. Median Δα 0.079 against 0.068 shuffled. Dollar-bar variance ∝ size^1.00 (R² 1.000), giving H ≈ 0.50 over the sampled scales. Choice of a forecasting clock remains open. | Descriptive scaling evidence · supplied “Proved” | Current notebook summary |

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-clk-01"></a>**CLK-01** · Follow-up | Dollar bars improve forecasts of the same next hour’s realised variance beyond the trading-rate information available in minute bars. | Fit identical models with clock-time or equal-notional histories. Also give the time model recent notional and activity. Evaluate on identical timestamps with the same future 60 clock minutes, matched lookback coverage and paired forecast losses. | Trades with price and size. Whole-minute approximation labelled separately | Extension of [DRAFT/N-16](#r-draft-n-16) and T-10. [T-10](#r-t-10), [T-11](#r-t-11), [DRAFT/N-16](#r-draft-n-16) |
| <a id="r-clk-02"></a>**CLK-02** · Follow-up | The excess multifractal width survives heavy tails, finite samples and changes of volatility regime. | Freeze scale range and q range, then compare Δα with a distribution of equal-length shuffled and phase-randomised, amplitude-adjusted surrogates. Repeat within regimes and on held-out dates. Estimate intervals for the width difference. | Trade or dollar-bar returns | Proposed from T-11. [T-11](#r-t-11) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-st-01"></a>**LOG/ST-01** | In standard units the coins have the same distribution, and the hour of day drops out | Quantiles of raw and standardised returns, volume and ranges per coin; spread of each quantile across the six coins relative to its size; hour-of-day profile. | Yes for returns, mostly for volume. The 1% and 99% points of z differ across coins by 2% to 3% of their size (raw returns: 33% to 35%), the 0.1% points by 7% to 10% (raw: 34% to 36%). Relative volume agrees within 2% to 9% above the median; below it the thin coins still differ by 23% to 31% (more near-empty minutes). BTC by hour: the raw absolute return swings from 0.80 to 1.38 of its daily mean and raw volume from 0.75 to 1.56; z2 stays within 0.97 to 1.02 and relative volume within 0.96 to 1.05. | Supported in the source test | HYP |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-draft-n-16"></a>**DRAFT/N-16** | A fixed pattern of price/volume behaviour predicts the next hourly outcome more consistently in equal-notional-volume bars than in equal-clock-time bars. Shortening bar completion times may add further information. | Construction — Set each asset's notional threshold from prior complete days; for example, the prior 20-day median daily quote notional divided by 288. Freeze it for the next day. The numerical target is a starting choice, not an optimum. Minute-only approximation — Aggregate whole completed minute bars until the threshold is crossed. Keep overshoot and elapsed time. Do not split or reorder unknown intraminute trades. Exact volume bars require trade-level data. Features — Apply a prespecified price-pattern feature to the completed volume bars; include bar duration relative to normal duration for that time of week. Outcome — The same next 60/240 clock minutes used by the time-bar model. Comparing next five volume bars with next five minutes would confound the horizon. Null — Time-bar features with equivalent elapsed-history coverage and controls for recent volume/volatility; compare at the same real decision timestamps. | Untested · Pasted text(20261124-205427).txt §8 proposed additions |
| <a id="r-draft-n-19"></a>**DRAFT/N-19** | The same prespecified mapping from normalised features to outcomes works on assets absent from model fitting, in a later test period. | Test — Compare volatility units, robust seasonal z-scores, and trailing percentile ranks using the same model, feature set and temporal splits. Hold out complete assets AND later dates. The held-out asset's own past may update an unsupervised scaler; its outcomes cannot be used to tune the mapping. Outcomes — Incremental predictive value, calibration, and effect size across held-out assets and regimes. Report market-factor and asset-specific results separately. Null — Per-asset models, raw-feature pooled models, and M0. Similar-looking charts without held-out outcome improvement do not pass. | Untested · Pasted text(20261124-205427).txt §8 proposed additions |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/IND-16, ALPHA/E24, ALPHA/SC-01

## Add a hypothesis

| ID | Hypothesis | Test / benchmark | Result | Status | Source |
| --- | --- | --- | --- | --- | --- |
| New unique ID | One claim that could fail | Observable trigger at t, target horizon, comparison group, effect measure |  | Proposed | Author, parent ID or source row |

When recording a run, add the date, dataset and universe, independent dates/events, effect and interval, and any competing outcome. For a revised definition, give the variant a suffix and retain the earlier result.

