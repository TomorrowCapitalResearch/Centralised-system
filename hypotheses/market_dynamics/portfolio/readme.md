# Portfolio exposure and joint risk

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**What happens when signals and positions share capital?** Joint tail loss, simultaneous fills, diversification and exposure-matched routing.

The earlier jump-state routing test failed on later splits. Only 5 of 32 other gates beat their unfiltered account in both later periods, before correcting for model selection. Joint fill or routing comparisons need the same funded capital and exposure. (Market State, “What the evidence already says”, Alpha tested #11/36.)

**Test design.** Use simultaneous market paths, a common capital budget and comparable gross exposure. A multi-asset fill episode is one common risk event. Measure joint losses, concentration and time spent in drawdown.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-port-01"></a>**PORT-01** · From research notes | Cross-coin passive bids fill together during shocks more often than a marginal fill model predicts. | Estimate marginal per-asset fill probabilities, then compare observed numbers of simultaneous fills and funded 1 h tail losses with an independent-fill baseline and a shared BTC-state model. Preserve episode-level dependence in uncertainty estimates. | Synchronized multi-coin submissions and funded fill replay | Framework FND-007, State M4 and [LOG/X4-d](10_cross_asset_dependence_and_contagion.md#r-log-x4-d). [LOG/X4-d](10_cross_asset_dependence_and_contagion.md#r-log-x4-d), [EXE-02](14_execution_fills_and_inventory.md#r-exe-02) |
| <a id="r-port-02"></a>**PORT-02** · Follow-up | A regime-routed strategy portfolio improves net outcomes beyond the effect of simply cutting exposure. | Freeze routing before evaluation. Compare against constant weights and an equally exposed schedule with matched turnover and capital limits. Score marked net returns, drawdown and time invested on later dates. | Causal state history and contemporaneous strategy return/fill streams | QRD H8/H9 and State evidence table. [REG-01](12_regime_transitions_and_structural_breaks.md#r-reg-01), [EXE-02](14_execution_fills_and_inventory.md#r-exe-02) |
| <a id="r-port-03"></a>**PORT-03** · New question | Staggering entries from highly correlated signals changes tail loss even at the same daily notional and average exposure. | With the same signals, compare immediate execution to a fixed staggered schedule and equally delayed control. Match realised exposure where feasible, include foregone signals and compare 1 h expected shortfall and net performance by common market episode. | Timestamped signals, funded order replay and synchronized returns | New timing-of-exposure comparison. [PORT-01](#r-port-01), [PORT-02](#r-port-02) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-bt-20"></a>**LOG/BT-20** | Portfolio of the relative-value fade (equal weight, one bar late) |  | Both sides: 5.6% a year at zero cost, -3.6% at 9 bps. Long only: 13.6% falling to 8.7% (Sharpe 0.73 to 0.51), which includes market drift. | Weak or mixed | NB |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-11, ALPHA/TEST-22, ALPHA/TEST-23, ALPHA/TEST-36, ALPHA/V6, ALPHA/IND-09, ALPHA/H8, ALPHA/H9, ALPHA/E10, ALPHA/E18, ALPHA/TR-01, ALPHA/C1, ALPHA/C5
