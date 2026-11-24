# Relative value, basis and venue leadership

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**Which leg moves when a spread closes?** Factor residuals, cross-venue prices, spot–futures lead–lag and executable parity.

In the rolling factor-gap study, nine comparisons indicated closing, six widening and 759 were inconclusive by its robustness rule. After one day, hedged fades lost before costs on 18/21 coins. The BTC futures lead appeared over three common book days. (LOG/X3-a, LOG/X3-b, LOG/F1-a, NB.)

**Test design.** Fix hedge weights, units, common quote currency and price timestamp at entry. Attribute spread closure to each leg, FX conversion and any reference update. Use simultaneous executable sides when judging a trade.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-rvl-01"></a>**RVL-01** · Follow-up | A factor residual’s apparent closure at one hour contains a measurable return on the initially hedged position. | Freeze beta and hedge at signal time. Attribute gap changes to coin, factor and subsequent refits. Compare fixed-hedge return with unhedged return and the moving model residual at 1 h and 24 h, by liquid coin and era. | Aligned factor/coin prices and executable hedge costs for economic tests | Extension of LOG §11.3, Framework FND-022 and Alpha E04. [LOG/X3-a](#r-log-x3-a), [LOG/BT-18](14_execution_fills_and_inventory.md#r-log-bt-18), [LVL-03](05_price_levels_revisits_and_reversion.md#r-lvl-03) |
| <a id="r-rvl-02"></a>**RVL-02** · Follow-up | An apparent spot–futures or inter-venue lead survives the information delay of the slower feed. | On fresh two-sided quotes compare direction and basis closure at matched receive times, symmetric artificial delays and reversed leader/follower roles. Evaluate 250 ms/1 s/5 s where resolution permits, estimate per day, and attribute which leg moved. | Synchronized local and leader BBO with event and receive times | Extension of LOG/F1-a/b and Framework FND-012. [LOG/F1-a](#r-log-f1-a), [LOG/F1-b](#r-log-f1-b), [LOG/X2-a](10_cross_asset_dependence_and_contagion.md#r-log-x2-a) |
| <a id="r-rvl-03"></a>**RVL-03** · From research notes | Same-asset prices in different quote currencies exhibit convergence beyond contemporaneous FX conversion. | Take synchronized executable quotes for the coin pairs and an independent FX cross. Freeze a signal and measure which leg closes the converted gap over 1/5 min. Compare with matched spreads and stale-quote placebos. Record fees and depth separately. | Timestamped venue prices, FX bid/ask, common size | Alpha M2, desk RV-03 and FX manual, spot cross rates. [RVL-02](#r-rvl-02), [EXT-02](13_derivatives_positioning_and_external_events.md#r-ext-02) |

## Earlier research in this category

### Historical study rows

Source wording is retained below. A bare ID inside quoted study text refers to the older log. NB, HYP, NOTE and PREV are defined in the source guide.

| Historical ID | Source question or run | Source test | Reported result | Source assessment | Evidence code |
| --- | --- | --- | --- | --- | --- |
| <a id="r-log-x3-a"></a>**LOG/X3-a** | The gap to β × BTC, or to β × market, closes (κ above 0) | 5-minute bars, β from a trailing 3-day regression, slope of the next W bars' gap on today's gap D | Mostly no: 9 robust closing, 6 robust widening, 759 not robust. Over 1 hour it closes on a few coins (LTC z 3.3, NEAR 3.9, XRP 3.4, DOGE 2.6 to 3.2) and widens on TRX (z -5.6), AURORA and ETH. Over 1 day the sign is mostly negative. | Unsupported in the source test | NB |
| <a id="r-log-x3-b"></a>**LOG/X3-b** | Fading a gap of 2 or more pays | Return of holding -sign(D) × (coin - β × BTC) for W bars | 1 hour: positive on 16 of 21 coins, +0.2 to +10 bps per trade before costs (NEAR 10.0, CRO 9.4, DOGE 8.2, XRP 6.6), but robust on only 2; negative on TRX, AURORA, BNB, ETH, FIL. 1 day: -9 to -156 bps on 18 of 21 coins. | Weak or mixed | NB |
| <a id="r-log-x3-c"></a>**LOG/X3-c** | The gap closes faster when the factor is calm | Slope when BTC's volatility is below against above its median | No consistent difference. | Unsupported in the source test | NB |
| <a id="r-log-f1-a"></a>**LOG/F1-a** | Futures lead spot | BTC order-book mids on 250 ms steps, 3 common days | Slope 0.31 at 0.25 s (z 69) and 0.26 at 1 s and 5 s. One step late: 0.07 to 0.09 (z 9 to 31). Spot to futures: 0.01 to 0.02. Worth 0.2 to 0.3 bps per step before costs. | Supported in the source test | NB |
| <a id="r-log-f1-b"></a>**LOG/F1-b** | One side closes the basis | Basis against its 15-minute average | Basis mean -4.5 bps, sd 0.71 bps, half-life about 3.8 seconds. The closing tests returned unstable slopes (hundreds to thousands, the two halves far apart). | Inconclusive | NB |
| <a id="r-log-f1-c"></a>**LOG/F1-c** | A jump on one side is followed on the other | Bars of 4σ or more | After a spot jump, futures continue 0.4 to 0.7σ (z 4 to 19). Several futures-jump rows blew up. | Inconclusive | NB |
| <a id="r-log-f1-d"></a>**LOG/F1-d** | The same on 1-minute futures bars over years | Needs futures klines | No futures klines in the data folder. | Untested or no recorded result | NB |

### Earlier proposals and unnumbered ideas

| Historical ID | Question | Original comparison or inputs | Status / provenance |
| --- | --- | --- | --- |
| <a id="r-backlog-n-08"></a>**BACKLOG/N-08** | The 1-hour gap to β × BTC closes on a fixed set of coins (LTC, NEAR, XRP, DOGE) and not on the rest | Freeze the list, then test on data after 2026-10-24 with a maker entry. | Proposed in the older log · X3-a |
| <a id="r-log-idea-05"></a>**LOG-IDEA/05** | Large volume on a drop or jump as participation when price is off its relative value; low volume at the "correct" price. |  | Open in source · Pasted text(20261124-205427).txt §12A |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/TEST-25, ALPHA/TEST-34, ALPHA/M1, ALPHA/M2, ALPHA/M3, ALPHA/Track-2a, ALPHA/LEGACY-N3, ALPHA/LEGACY-N4, ALPHA/E04, ALPHA/RV-01, ALPHA/RV-02, ALPHA/RV-03, ALPHA/C2
