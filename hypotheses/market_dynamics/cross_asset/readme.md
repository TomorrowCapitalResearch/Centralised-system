# Cross-asset dependence and contagion

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**Which disturbances spread across assets?** Common factors, breadth, residual contagion, tail dependence and aggregation.

The study log records a +3.7 percentage-point excess chance of a crash within an hour for coins that had escaped the initial BTC crash, robust on 19/21 coins. Excess downside relative to upside was robust on 8/21 at that horizon. A forecast of disturbance and a forecast of its sign deserve different rows. (LOG/X4-b, LOG/X4-c, NB.)

**Test design.** Align timestamps and data availability. Exclude the target coin from peer breadth and market factors where feasible. Keep assets together in calendar blocks when estimating uncertainty. Report liquid names and stale-price sensitivity.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-xas-01"></a>**XAS-01** · Follow-up | A target coin’s next-shock probability rises with peer shocks even after BTC, ETH and common-market shocks are known. | Exclude the target from breadth and the factor. At t add lagged residual peer shocks to own state plus BTC/ETH. Compare next-15-min event loss and calibration. Use common-date bootstrap and delay all peers by one bar. | Aligned liquid-coin 1-min panel | Extension of [DRAFT/N-17](#r-draft-n-17), [BACKLOG/N-16](#r-backlog-n-16) and [LOG/SE-09](#r-log-se-09). [LOG/SE-09](#r-log-se-09), [LOG/X4-b](#r-log-x4-b) |
| <a id="r-xas-02"></a>**XAS-02** · From research notes | A calm basket can mask high constituent dispersion that precedes a later broad volatility increase. | In a fixed-weight basket compute weighted squared constituent returns minus squared basket return. Match current basket volatility, BTC volatility and breadth. Test whether past internal dispersion improves next-hour basket variance out of sample. Keep weights point-in-time. | Aligned constituent returns and predeclared weights | Aggregation Theory Part II §§3, 6 and QRD H7, forecasting extension. [LOG/X1-b](#r-log-x1-b), [VOL-03](07_volatility_ranges_and_turbulence.md#r-vol-03) |
| <a id="r-xas-03"></a>**XAS-03** · New question | Shock propagation differs between equally broad sell-offs that begin in liquid leaders and those that begin among smaller coins. | Define the origin from the first completed 5-min block only. Hold breadth, shock magnitude and BTC return fixed. Score the fraction of as-yet-unshocked coins hit in the next 15 min. Exclude ambiguous simultaneous origins in a sensitivity run. | Point-in-time universe, liquidity ranks, aligned minute bars | New origin-versus-breadth comparison. [LOG/X4-b](#r-log-x4-b), [LOG/SE-09](#r-log-se-09) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-x1-a"></a>**LOG/X1-a** | Returns are correlated across assets | Correlation at 5 minutes to 1 day | With BTC: 0.52 to 0.73 for most coins at every horizon, ETH 0.82 to 0.85. Thin coins rise with the horizon (AURORA 0.07 to 0.38, CRO 0.47 to 0.62). FX about 0 to 0.16, metals 0.13 to 0.26. Mean coin-to-coin 0.56 to 0.59. | Supported in the source test | NB |
| <a id="r-log-x1-b"></a>**LOG/X1-b** | Coins are tied to BTC more in crashes than in rallies | Share of BTC's worst 5% bars that are also the coin's worst 5%, against the same for the best 5% | 1-hour bars: 47% to 58% in the lower tail (ETH 67%) against 35% to 47% in the upper tail and 28% to 42% implied by correlation alone. Lower minus upper is +10 to +15 pp (z 7 to 10), and +16 to +36 pp on daily bars. Robust on 19 of 21 coins. Nothing in FX or metals. | Supported in the source test | NB |
| <a id="r-log-x2-a"></a>**LOG/X2-a** | BTC leads the other coins at 1 minute | Slope of the coin's next bar on BTC's last bar, own bar held fixed; then one bar late | At once: every coin (z 11 to 59). One bar late: about zero for liquid coins (z -2.0 to 5.8), large only for AURORA (19.2) and CRO (24.4), whose closes are unchanged in 51% and 34% of minutes. Following BTC's sign earned 0.1 to 0.5 bps on the next minute before costs. | Weak or mixed | NB |
| <a id="r-log-x2-b"></a>**LOG/X2-b** | Other assets lead BTC | The same in reverse | ETH leads BTC more than it follows (z -5.3). Gold, silver and GBPUSD lead BTC at 1 and 5 minutes more than they follow (z -2.2 to -6.2). | Supported in the source test | NB |
| <a id="r-log-x2-c"></a>**LOG/X2-c** | After a BTC jump (a 1-minute bar of 3σ or more) the others continue and catch up | 34,780 jumps. Catch-up = the coin's continuation minus β × BTC's own | +0.6 to +1.7 bps of catch-up in the next minute (z 7 to 16), up to about 2.6 bps over 5 to 60 minutes. Robust on 19 of 21 coins for the next 1 and 5 minutes, 17 to 18 for 15 and 60. Mostly gone for 5-minute jump bars (10 of 21). | Supported in the source test | NB |
| <a id="r-log-x4-a"></a>**LOG/X4-a** | When BTC crashes (a 1-hour bar beyond 3σ down) the others crash in the same hour | Hourly panel | 31% to 50% of the time (ETH 63%) against a usual 0.7% to 1%. | Supported in the source test | NB |
| <a id="r-log-x4-b"></a>**LOG/X4-b** | After a BTC crash, coins that did not crash follow later | Excess over the usual rate | +3.7 pp within 1 hour (robust on 19 of 21), +6.2 pp within 6 hours (16 of 21), +6.6 pp within 24 hours (13 of 21). | Supported in the source test | NB |
| <a id="r-log-x4-c"></a>**LOG/X4-c** | It spreads as a crash: down more than up | Excess for down-jumps minus excess for up-jumps | Robust on only 8 of 21 within 1 hour and 2 of 21 within 6 hours. | Weak or mixed | NB |
| <a id="r-log-x4-d"></a>**LOG/X4-d** | A broad crash (a quarter of the coins in one hour) is followed by another | 467 broad crashes | Another one is +5.9 pp more likely within 1 hour (z 5.3), +9.5 pp within 6 hours (z 6.0), +11.0 pp within 24 hours (z 5.3). Broad crashes repeat more than narrow ones: +4.6 pp within 1 hour (z 4.0). | Supported in the source test | NB |
| <a id="r-log-x4-e"></a>**LOG/X4-e** | FX and metals jump when crypto crashes | Same hour and the next | +5.6 to +6.7 pp in the same hour (z 3.3 to 3.7), almost all of it from the second half of the sample. | Weak or mixed | NB |
| <a id="r-log-x5"></a>**LOG/X5** | Net taker buying in BTC predicts the other coins beyond both prices | Slope on the coin's next L minutes, given both returns and the coin's own flow | It predicts lower returns: about -0.009 over 15 minutes (robust on 14 of 19 coins) and -0.016 over 60 minutes (19 of 19). Nothing at 5 minutes. | Opposite sign in the source test | NB |
| <a id="r-log-se-09"></a>**LOG/SE-09** | Shocks in other coins raise this coin's chance of a shock, and some coins move first | Number of other coins with a move of 3+ in the last 5 minutes: a main shock here within 5, 15, 60 minutes, at the same own volatility and own recent shock. Net direction of the others' shocks: return here. Market-wide shocks (half the coins within 3 minutes): who is first. `hyp_xseis` | Yes, 6 of 6. With 2 or more other coins shocked, a main shock here in the next 15 minutes is 4.8 pp more likely (z 33.5); with half of them, +6.4 pp; still +8 to +9 pp over the next hour. Direction carries over weakly: +0.011σ over 5 minutes (z 4.0). In market-wide shocks BTC and ETH are among the first movers 60% of the time, XRP, SOL and ADA 47% to 48%, TRX 31%. | Supported in the source test | HYP |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-backlog-n-16"></a>**BACKLOG/N-16** | Shocks in other coins are an earlier warning than the coin's own volatility | Add the count of other coins shocked in the last 5 minutes to the regime engine's `risk` axis; score the hazard out of sample. | Proposed in the older log · SE-09 |
| <a id="r-draft-n-17"></a>**DRAFT/N-17** | A rising share of liquid assets experiencing small shocks predicts a target asset's next large move beyond its own state and BTC/ETH activity. | Features — In each completed five-minute interval, calculate the fraction of eligible peers with upward and downward microshocks, change in these fractions over 30 minutes, and concentration in a few leaders versus broad participation. Exclude the target from its peer signal. Outcome — Target-asset jump probability and direction over the next hour; market-wide event probability as a distinct secondary outcome. Null — Own state, BTC and ETH returns/volatility/shock counts, and a trailing market factor. Run raw and market-residual versions; both can be useful, but they answer different questions. Data controls — Common UTC close times, source latency where known, stale-price filters, and universe membership decided from information then available. AURORA/CRO should be excluded from the initial liquid-universe test described in your log. Resample all assets jointly by time; correlated coins are not independent replications. | Untested · Pasted text(20261124-205427).txt §8 proposed additions |
| <a id="r-log-idea-22"></a>**LOG-IDEA/22** | Your own cross-asset rule. |  | Open in source · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-19, ALPHA/TEST-20, ALPHA/TEST-26, ALPHA/T6, ALPHA/Track-2b, ALPHA/IND-17, ALPHA/H7
