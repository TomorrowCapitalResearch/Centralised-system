# Regime transitions and structural breaks

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**How long does a state last, and can its exit be forecast?** Dwell time, change alarms, stability, transition paths and delayed labels.

The earlier jump-state routing portfolio had train/validation/test Sharpes of 2.20, −0.26 and −0.16. Direction-state labels also had confirmation delays. A forecast of the next state needs its own forward target. (Market State, “What the evidence already says”.)

**Test design.** Define the current state using observations available at the time and freeze the labelling rule before evaluation. Record alarm time and confirmation delay. Score current-state description, future transition and policy value independently.

### Your current rows

| ID | Hypothesis | Test or next comparison | Reported result and scope | Assessment | Source |
| --- | --- | --- | --- | --- | --- |
| <a id="r-t-07"></a>**T-07** | Major bull and bear periods alternate with a semi-regular cycle length. | Use the same turning-point rule on actual and bootstrap random-walk paths. Compare the dispersion of cycle lengths. | 5/6 assets had measurable cycles. None of those five was more regular than the null. Median cycle 131 days, coefficient of variation 0.41 against 0.52 under the null. | No support in sample · supplied “Disproved” | Current notebook summary |

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-reg-01"></a>**REG-01** · Follow-up | Knowing the time already spent in a filtered state improves the probability of exit over the next day. | Define volatility states from trailing data and freeze thresholds. Compare a constant-transition Markov model with a duration-dependent hazard controlling current volatility. Score first exit, 24 h Brier loss and calibration. Run directional trend states as a distinct target. | Minute/hourly returns | QRD §§5–6, extension of T-07 and [LOG/VO-02](07_volatility_ranges_and_turbulence.md#r-log-vo-02). [T-07](#r-t-07), [LOG/VO-02](07_volatility_ranges_and_turbulence.md#r-log-vo-02) |
| <a id="r-reg-02"></a>**REG-02** · From research notes | Increasing return-to-baseline time after small perturbations provides warning of a structural break beyond volatility acceleration alone. | Estimate response decay using only past shocks. Compare a CUSUM/variance baseline with and without recovery time. Evaluate median warning lead and false alarms per 100 days against a frozen future break label. | Minute or hourly returns and causal alarm timestamps | QRD §§8, 31, 43 and H4. [LOG/TD-03](#r-log-td-03), [JMP-01](08_jumps_precursors_and_recovery.md#r-jmp-01) |
| <a id="r-reg-03"></a>**REG-03** · New question | Frequent disagreement between fast and slow state estimates precedes higher transition uncertainty, even when both estimates have the same current label. | On completed hours with matching current labels compare previous 6 h disagreement counts. Control current state probabilities, realised risk and state age. Predict a frozen next-6 h state and assess log loss and calibration against a state-age baseline. | Causal fast/slow state histories and returns | New state-history comparison informed by QRD H12. [REG-01](#r-reg-01), [LOG/TD-03](#r-log-td-03) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-td-03"></a>**LOG/TD-03** | A change-point alarm (CUSUM) on 15- or 60-minute returns marks the start of a trend | Alarm when the cumulative standardised return drifts 4 or 6 beyond its allowance: return in that direction over the next 4 to 24 bars. | Weak. After an alarm the next 12 bars continue: +0.046 to +0.065σ on 15-minute bars (z 3.1 to 3.2), +0.067 to +0.10σ on hourly bars (z 2.6 to 2.9). Positive on 5 to 6 of 6 coins, robust alone on at most one. 4 of 12 pooled rows. | Weak or mixed | HYP |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-draft-n-15"></a>**DRAFT/N-15** | A causally detected change in return drift, supported across short and longer windows, predicts remaining movement over the next 1–4 hours. | Features — An online two-sided cumulative-sum detector on standardised returns; signed returns/efficiency over 15, 60 and 240 minutes; an explicit record of the first detection time. Set the detector threshold using training data and a chosen false-alert rate. Begin with one detector and a small fixed set of horizons. Outcome — Remaining signed return from the observable detection time, trend-label probability, and adverse excursion. Compare at completed hourly endpoints as the primary analysis, then study minute-level updates separately. Null — Ordinary 1-hour and 4-hour momentum, matched on signal size and volatility; assess drift detection and cross-horizon agreement separately. Timing — Never move the signal timestamp back to the low/high that was identified only afterwards. Detection delay is part of the result. | Untested · Pasted text(20261124-205427).txt §8 proposed additions |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-06, ALPHA/TEST-09, ALPHA/TEST-10, ALPHA/TEST-14, ALPHA/TEST-15, ALPHA/TEST-16, ALPHA/TEST-18, ALPHA/TEST-33, ALPHA/LEGACY-N5, ALPHA/H1, ALPHA/H4, ALPHA/H11, ALPHA/H12, ALPHA/E06, ALPHA/E22
