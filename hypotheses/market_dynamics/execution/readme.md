# Execution, fills and inventory

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**What happens to an order and the inventory it creates?** Passive fills, adverse selection, quote placement, inventory adjustment and opportunity cost.

The archive’s 20-coin, 12σ ladder had reported Sharpe 1.71 under the original fill convention, 1.17 at 5 bp trade-through and 0.63 at 20 bp. Cash reservation, marking and arrival/fill assumptions limit comparisons. (Alpha tested #1–3, Framework FND-005/006.)

**Test design.** Reserve cash when an order rests, mark open inventory, and model the same arrival and cancellation delays across policies. Report all submitted orders, filled quantity, missed opportunities, markouts and forced exits.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-exe-01"></a>**EXE-01** · Follow-up | A wick ladder’s apparent edge survives conservative fills and the cost of reserving cash. | Freeze the 12σ/4 h specification from the archive. Compare touch, trade-through and queue-aware replay on the same funded order stream. Score net P&L per submission and per fill, time-weighted inventory and missed orders. Keep adverse exits. | OHLC for bounds. Sequenced books/trades for fill claims | Alpha tested #1–3, Framework FND-005/006. [LOG/BT-12](#r-log-bt-12), [LOG/BT-14](#r-log-bt-14) |
| <a id="r-exe-02"></a>**EXE-02** · From research notes | After market turbulence, widening quotes or reducing inventory limits improves downside outcomes over a fixed policy at equal average exposure. | Replay the same signals with a frozen lagged turbulence measure, ordinary volatility, and a matched static size reduction. Compare net markout, aggregate fill rate, marked drawdown and inventory tail across the same days. | Funded order/fill replay, aligned returns and covariance history | State M2–M5, Framework FND-007/010/011 and desk MM-01. [EXE-01](#r-exe-01), [PORT-01](15_portfolio_exposure_and_joint_risk.md#r-port-01) |
| <a id="r-exe-03"></a>**EXE-03** · New question | The opportunity cost of missing passive fills is higher after a breakout that holds than after a failed breakout. | From the same eligible submissions, include zero-fill and partial-fill paths. Compare arrival-price benchmark shortfall and subsequent mid returns at 30 min across states defined at submission. Fix side, size, price distance and latency. | Timestamped submissions or replay with books and trades | New execution comparison informed by Alpha E14. [TRD-01](06_trend_breakouts_and_price_paths.md#r-trd-01), [EXE-01](#r-exe-01) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-bt-01"></a>**LOG/BT-01** | Baseline: length 5, up-then-down, quote 1 bp away, fill at touch |  | -81.6% against +78.1% for buy and hold. 7,360 trades, 56.2% wins, -2.2 bps per trade, buy markout -1.15 bps. | Unsupported in the source test | NB |
| <a id="r-log-bt-02"></a>**LOG/BT-02** | Zero offset |  | Length 5 +60.0%, length 7 about +144% (22,553 trades, +0.42 bps per trade, daily Sharpe 2.79), length 10 +106.9%. A cost-free bound. | Weak or mixed | NB |
| <a id="r-log-bt-03"></a>**LOG/BT-03** | Any price improvement needed |  | Offsets of 0.5 to 5 bps lose 44% to 100%. Fill only when price trades 1 bp through: -93.0%. 3 bps through: -96.8%. Always quoting: -99.8% or worse. | Unsupported in the source test | NB |
| <a id="r-log-bt-04"></a>**LOG/BT-04** | Fill-conditioned policy, length 10 |  | The one positive family with an offset: +10% to +57% at 0.5 to 2 bps (1,087 to 1,681 trades, +1.7 to +4.2 bps per trade), -2% at 5 bps. One family in a 105-row grid on the scored segment. Not checked with through fills or on the hold-out. | Inconclusive | NB |
| <a id="r-log-bt-05"></a>**LOG/BT-05** | Event study: a 1% drop on low volume (lowest 10%) is a gap that refills |  | Such drops barely exist in the majors (BTC 0 to 1 raw cases, ETH 14). Where they do, recovery is more frequent (DOGE 77% to 85%, SOL 65% to 68%, against 58% to 63% for other drops), but the mean return over the horizon is often negative, and only a quarter to a third of 1% drops recover before a 50 bp stop. | Weak or mixed | NB |
| <a id="r-log-bt-06"></a>**LOG/BT-06** | Buy every 1% drop: bid 1% under the last close, target +0.5%, stop -0.5% |  | BTC: 779 trades, 46.3% wins, -9.5 bps per trade, -53%. ETH -86%, DOGE -100%, SOL -99.8%. With touch fills and no slippage BTC is still -7.6 bps per trade at 47.1% wins. Break-even at these exits is about 55%. | Unsupported in the source test | NB |
| <a id="r-log-bt-07"></a>**LOG/BT-07** | The same, only on low-volume drops |  | 5 to 48 trades per asset (BTC +5.6 bps on 5, SOL +7.5 on 47, ETH -5.5 on 14, DOGE -3.2 on 48). Too few to judge. | Inconclusive | NB |
| <a id="r-log-bt-08"></a>**LOG/BT-08** | Volatility-scaled distances (ATR), and a threshold search |  | Every ATR variant is negative (-1.6 to -11.2 bps per trade). The search over 39 candidates ended with "no candidate passed train/validation requirements". | Unsupported in the source test | NB |
| <a id="r-log-bt-09"></a>**LOG/BT-09** | Smaller and larger drops (exploratory, BTC) |  | After 0.5% drops, low-volume recovery is 72% to 85% against 61% to 68% for other drops (15 minutes, lowest 10%: 77.8%, interval 69.4 to 84.4, n 117). Payoff mixed: -13 to +21 bps. Deeper drops recover less often: 47% to 56% after 1%, 40% to 52% after 1.5%. | Weak or mixed | NB |
| <a id="r-log-bt-10"></a>**LOG/BT-10** | Bid distance under the rolling low |  | Median out-of-sample Sharpe is negative at every distance except 2% under the lowest (+0.12, 57.8% of setups positive). Window, and low against close, change little. | Weak or mixed | NB |
| <a id="r-log-bt-11"></a>**LOG/BT-11** | A 0.45% stop |  | With the stop: 37.6% wins, -2.06 bps per trade, out-of-sample Sharpe -0.90. Without: 87.9% wins, +40.7 bps per trade, out-of-sample Sharpe +0.09, but 75% exposure, 41% drawdown and positions left open. The stop is better in 12% of 1,656 matched pairs (median change -1.11). | Unsupported in the source test | NB |
| <a id="r-log-bt-12"></a>**LOG/BT-12** | Fill rule |  | Touch +3.79 bps per trade, 1 bp through +2.06, close at the level -8.72, close through -10.13. | Weak or mixed | NB |
| <a id="r-log-bt-13"></a>**LOG/BT-13** | Target by volume state, or in units of σ |  | Wider targets earn more per trade (high-volume fills: +1.57 bps at 0.3%, +10.22 at 1%; σ targets: +0.31 at 1σ, +7.06 at 3σ), but the median out-of-sample Sharpe stays negative (-0.15 to -0.28). | Weak or mixed | NB |
| <a id="r-log-bt-14"></a>**LOG/BT-14** | The best in-sample setups carry forward |  | No. On BTC, in-sample Sharpe 1.3 to 1.8 becomes -0.75 to -7.2. 2021 carries the result; 2024 to 2026 are negative for the best three. A 9 bps fee on the stop turns +0.30 into -1.85 bps per trade. Top tenth, in sample to out of sample: BTC 0.88 to -0.57, ETH 0.65 to -0.21, DOGE 0.69 to -0.22, SOL 1.58 to +0.25. | Unsupported in the source test | NB |
| <a id="r-log-bt-15"></a>**LOG/BT-15** | Gold |  | Holds out of sample (top tenth 1.80 to 1.12; best setup 2.40 to 1.80, +2.78 bps per trade). The gold bars are mid prices, so no spread is charged. | Inconclusive | NB |
| <a id="r-log-bt-16"></a>**LOG/BT-16** | Regimes of the best BTC setup |  | Low-volume fills +1.90 bps (80.8% wins) against +0.15. Wild volatility +1.35 against calm -0.24. Europe 8 to 13 UTC +1.11. After a leader drop +0.73 against +0.12. Weekend +0.69 against +0.16. Whole sample, setup chosen in sample: leads, not results. | Inconclusive | NB |
| <a id="r-log-bt-17"></a>**LOG/BT-17** | Follow the leader after a BTC jump |  | The best rules bear 1 to 3.5 bps per side when entered at once. One bar late the family keeps about 10% (median 0.51 to 0.04 bps). | Unsupported in the source test | NB |
| <a id="r-log-bt-18"></a>**LOG/BT-18** | Fade after contagion; fade a 1-day relative-value gap |  | Highest break-even costs: contagion fade on 60-minute bars, hold 24 (13.6 bps median, -22 to +35 across coins, 0.2 trades a day), and relative value W=288 fade (11.2 bps, -55 to +53, positive in both halves on 6 to 7 of 20 coins). | Weak or mixed | NB |
| <a id="r-log-bt-19"></a>**LOG/BT-19** | What survives 9 bps per side |  | Pairs positive in both halves: follow 10 of 1,300 at once and 0 one bar late; relative value 13 of 288 at once and 10 one bar late; contagion 5 of 150; flow 0 of 208; reverse 0 of 450. About half of the best rows are AURORA (stale prices). | Unsupported in the source test | NB |
| <a id="r-log-bt-21"></a>**LOG/BT-21** | Resting-order entries, long side |  | Relative-value fade W=288: +91 to +95 bps per fill, but only 2.2% of signals fill (16 of 16 coins positive, Sharpe about 0.65). Contagion fade: +25 to +28 bps at 71% fills. Follow 5-minute jumps, hold 60: +10 to +12 bps at about 64% fills, with drawdowns of about 80% to 85%. Long only, so drift is included. | Weak or mixed | NB |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-backlog-n-01"></a>**BACKLOG/N-01** | The 1% dip bracket wins more often when the fill bar is on low volume | Win rate and bps by `participation` state, scored out of sample. Baseline to beat: 46% to 47% wins against about 55% needed (BT-06). | Proposed in the older log · BT-16 (+1.90 against +0.15 bps), BT-09 |
| <a id="r-backlog-n-02"></a>**BACKLOG/N-02** | The dip bracket does better in high-volatility states than in calm ones | The same split by `vol_level`. | Proposed in the older log · BT-16 (wild +1.35, calm -0.24) |
| <a id="r-backlog-n-03"></a>**BACKLOG/N-03** | Dip-buying in the 1 to 6 hours after a BTC crash or a broad crash loses more than usual | Split by hours since the last 3σ hourly down-jump in BTC, and by `risk` state. | Proposed in the older log · X4-b, X4-d |
| <a id="r-backlog-n-04"></a>**BACKLOG/N-04** | Rises fade and drops do not: a sell-the-rise bracket beats its mirror, a buy-the-drop bracket does not | Re-run the fade and continuation brackets on the 26 assets, both directions under one rule. | Proposed in the older log · P-05, P-06 |
| <a id="r-backlog-n-05"></a>**BACKLOG/N-05** | Heavy BTC taker buying over the last 15 to 60 minutes is followed by lower returns in the other coins, by enough for a maker to use | Use it as a filter on bids; score with touch and through fills. | Proposed in the older log · X5, V-25 |
| <a id="r-backlog-n-06"></a>**BACKLOG/N-06** | After a BTC 3σ minute, a resting order in the other coins captures the catch-up | Hedged instead of long only, one bar late, AURORA and CRO left out. | Proposed in the older log · X2-c, BT-21 |
| <a id="r-backlog-n-07"></a>**BACKLOG/N-07** | Targets and stops in units of current volatility beat fixed percentages | The same bracket with distances scaled by the volatility state, compared out of sample. | Proposed in the older log · PS-01, PS-03, BT-13 |
| <a id="r-backlog-n-11"></a>**BACKLOG/N-11** | After 2 or more shocks in 5 minutes, a resting bid is filled into a further shock more often than it earns: pulling or widening quotes for the next 15 to 60 minutes improves markout | Join the shock count to the fills of the maker backtest; compare markout and stop-outs with and without the filter, out of sample. | Proposed in the older log · SE-03, SE-07 |
| <a id="r-backlog-n-12"></a>**BACKLOG/N-12** | Quotes can come back on a schedule: the extra shock rate decays as lag^-0.67, so half of it is gone after about 3 minutes and three quarters after about 8 | Re-entry delay of 1, 3, 8, 20 minutes after a shock, same backtest. | Proposed in the older log · SE-01 |
| <a id="r-backlog-n-14"></a>**BACKLOG/N-14** | A band break (P-22, P-23) is a usable skew for a maker: quote one side only for 10 to 30 minutes | Markout of bids against asks after breaks, one bar late, touch and through fills. | Proposed in the older log · P-22, P-23 |
| <a id="r-backlog-n-15"></a>**BACKLOG/N-15** | Buying 2 sd under the 20-bar average works with a small target and no tight stop, and fails as hold-for-h | Bracket grid on the same events; compare with the hold-for-h mean. | Proposed in the older log · PS-13, check 13 |
| <a id="r-log-idea-21"></a>**LOG-IDEA/21** | Your own market-maker slot: more entry, exit and stop features. |  | Open in source · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-01, ALPHA/TEST-02, ALPHA/TEST-03, ALPHA/TEST-04, ALPHA/TEST-05, ALPHA/TEST-21, ALPHA/TEST-30, ALPHA/V5, ALPHA/H10, ALPHA/E11, ALPHA/E12, ALPHA/E14, ALPHA/E17, ALPHA/MM-01, ALPHA/MM-02, ALPHA/MM-04, ALPHA/C3, ALPHA/C4-RL-01

## Add a hypothesis

| ID | Hypothesis | Test / benchmark | Result | Status | Source |
| --- | --- | --- | --- | --- | --- |
| New unique ID | One claim that could fail | Observable trigger at t, target horizon, comparison group, effect measure |  | Proposed | Author, parent ID or source row |

When recording a run, add the date, dataset and universe, independent dates/events, effect and interval, and any competing outcome. For a revised definition, give the variant a suffix and retain the earlier result.
