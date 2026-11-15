
# fundamental idea

The fundamental idea in trading:
 **fair value depends on who else is in the market.** The asset is worth whatever the next counterparty will pay, and that depends on whether anyone shows up, how many show up, and how well informed they are.

## The core statement

> **Trading is the act of standing between counterparties whose valuations disagree, and capturing part of the gap. How big a gap exists, and how much of it you keep, depends on how much interest there is and how much competition you face.**

Or more compactly: *buy from those who value it less than the next buyer will; sell to those who value it more than the last seller did.*

## The two axes

Your three scenarios fit on two dimensions:

| | **Low competition** | **High competition** |
|---|---|---|
| **High interest** | *Price setter.* You're the only one supplying liquidity, so you set the spread and earn a rent. Your own impact is the dominant force. | *Error harvester.* The price is roughly efficient, and you profit only from mismatches between buyers and sellers. Your edge shrinks as more skilled players join. |
| **Low interest** | *Illiquid.* There's nobody to trade with. Price is undefined or meaningless, and any trade happens at a large discount or premium. | *Crowded and dead.* Many traders chase a few counterparties. Spreads collapse and nobody makes money. |

**Interest** sets how much flow exists. **Competition** sets who captures it.

## The errors you harvest in the competitive quadrant

These are the drifts and reversions you described. They come from three sources:

1. **Information errors.** A counterparty doesn't know something yet: news, an event, a structural fact.
2. **Behavioural errors.** A counterparty knows but reacts badly: overreaction, anchoring, herding, panic.
3. **Constraint errors.** A counterparty knows and reacts rationally, but is *forced* to trade at a bad price: margin calls, rebalancing, mandates, inventory limits, needing immediacy.

Each source shows up at different horizons. Constraint and behavioural errors dominate at short horizons (microstructure, flow). Information errors tend to play out at medium and long horizons (repricing on fundamentals, regime shifts).

## Two refinements

- **Errors are relative.** You don't need to be right. You only need to be *less wrong than the person on the other side*, by more than your costs and your own price impact. That means edge isn't absolute. It's a function of your counterparties, which closes the loop back to "it depends on your audience."
- **You are part of the market.** In the monopoly and thin-market cases, your own trading moves the exit price. Strictly, the question isn't "is this price good versus fair value?" It's "is this price good versus the price I can *exit* at, given that I'm the one exiting?"

## As a single expression

$$
\text{Edge} \approx \underbrace{\mathbb{E}[\text{exit price} \mid \text{future counterparties}] - \text{entry price}}_{\text{gap you capture}}  - \underbrace{\text{impact}(q, \text{depth})}_{\text{your own footprint}} - \text{costs}
$$

Here the expected exit price depends on interest (is anyone there?) and competition (how much of the gap survives others getting to it first?).

If you want formal anchors for this, two classic models from market microstructure each formalise part of it. Kyle (1985) covers informed traders versus noise traders and how price impact depends on depth. Glosten–Milgrom (1985) explains why spreads exist when some counterparties know more than others. Your framing essentially generalises both by letting participation and competition vary.


*Note: We are therefore looking to hit these ideas best*\

reference   v[i,t] = a[i] + b[i]·F[t]              b from a trailing window\
deviation   d[i,t] = p[i,t] − v[i,t]               scaled by its own trailing volatility\
state       s[t]                                   two or three states at most\
forecast    E[r(i, t→t+h)] = b[i]·E[r(F) | s] − (1 − exp(−κ(s)·h))·d[i,t]

# Our idea

with the above as our premise, we have the following conditions.
1. We must be able to get in and out of a trade, meaning we do not want to be in illiquid markets.
2. 

We ought to earn money because of:
1. We utilise market "inevitabilities" to earn money on holding, buy, selling between events that are bound to happen.
2. We utilise market "inefficiencies" to earn money on minimising errors in the markets.
3. We utilise market "matching" to earn money on providing liquidity


## market inevitabilities
Here we are considering things that WILL happen in the markets at some point, regardless of how much we try to reduce errors in prices.

## market inefficiencies
- price discrepancies in currency pairs
- price discrepancies in lead-lag between currencies
- price discrepancies in venue-lag
- relative value discrepancy within an asset
- price discrepancy based on information
- liquidity gaps

## market matching
market making, 




# Market dynamics
# Research hypothesis log

Every hypothesis in `research_upgraded.ipynb`: how it was tested, what came out, and what is still open.

- **Data in the notebook:** 1-minute bars through 2026-10-24
- **Compiled:** 2026-11-14
- **Convention:** add rows, don't delete them. When a result changes, update the status and keep the old number in the result cell.

## Contents

1. [How to read this](#1-how-to-read-this)
2. [Where things stand](#2-where-things-stand)
3. [Volume](#3-volume)
4. [Price](#4-price)
5. [Path study](#5-path-study)
6. [Volatility](#6-volatility)
7. [Cross-asset](#7-cross-asset)
8. [Backtests](#8-backtests)
9. [Contradictions and checks](#9-contradictions-and-checks)
10. [Backlog](#10-backlog)
11. [Template for a new hypothesis](#11-template-for-a-new-hypothesis)

---

## 1. How to read this

### Status

| Mark | Meaning |
|---|---|
| ✅ | Holds: robust in the test as it was run |
| 🟡 | Weak or mixed: small, only on some assets, or only before costs |
| ❌ | Does not hold |
| 🔁 | The opposite holds |
| 🔧 | Inconclusive: the test ran but needs fixing or more data |
| ⏳ | Not run, or no result on file |

### Where each result comes from

| Code | Meaning |
|---|---|
| `NB` | Output stored in this notebook copy, run on up to 26 assets: 22 coins, GBPUSD, USDJPY, XAGUSD, XAUUSD. |
| `NOTE` | Your own conclusion written in the notebook. No table is stored with it. |
| `PREV` | Earlier run on BTC, ETH, DOGE, SOL, EURUSD and XAUUSD, read off your scoreboards on 12 and 13 Nov (chats "Volume and price revisit analysis" and "Crypto volume-price dynamics research"). Not stored in this copy and not repeated on the 26 assets yet. |

### What this copy of the notebook contains

| Section | Stored results |
|---|---|
| Data and the data-kit test | Yes |
| Volume (9 suites) | No. Only the taker-flow test on the flow files |
| Price (3 suites) | No |
| Path study | Yes |
| Volatility | No |
| Cross-asset (X1 to X5, F1, B1 to B3) | Yes |
| Everything on one board | Cross-asset rows only: 2,457 tests |
| Backtests (pattern, liquidity gap, market maker, cross-asset) | Yes |

The "what is already computed" table shows only `xa_pair` and `xa_relval` as done. Thirteen suites show "-", so sections 3, 4 and 6 rest on your notes and on the earlier run. EURUSD could not be read in this run (`No timestamp column/index found`).

### Verdict rule

The notebook counts an effect when its Benjamini-Hochberg q is at most 0.05 inside its family **and** the sign is the same in both halves of the history. The older volume and price boards use an absolute z of 3 or more with both halves agreeing. Rows that only describe same-bar co-movement never count.

### Data

- 20 coins: 2021-01-01 to 2026-10-24, about 3.06 million 1-minute rows each.
- AURORAUSDT (from 2024-06, 36% of minutes with zero volume) and CROUSDT (from 2024-01, 30%) are thin.
- GBPUSD, USDJPY, XAGUSD, XAUUSD: 2023-01 to 2026-10-01, mid prices, tick volume.
- Taker-flow files for BTC, ETH, SOL: 2023-01 to 2026-08.
- Order books: BTC spot 4 days, BTC futures 3 days.

---

## 2. Where things stand

95 rows are logged below: 20 hold, 26 are weak or mixed, 24 do not hold, 4 show the opposite, 10 are inconclusive and 11 have not been run.

The model in the first two cells: a reference value `v = a + b·F`, the deviation `d = p - v`, a state `s`, and a forecast in which the deviation closes at speed `κ(s)`.

**Holds so far**

- Volume and absolute return move together in the same bar (V-05). Volatility persists: quiet stays quiet and expansion stays wide (PS-01, PS-03).
- Coins are tied together more in crashes than in rallies (X1-b). Crashes cluster in time and spread across coins (X4-b, X4-d).
- A BTC jump carries into the other coins for up to an hour, by about 1 bp (X2-c).
- Taker flow is positive for the next minute and negative over 15 to 60 minutes, within a coin and from BTC to the others (V-25, X5).
- Futures lead spot at sub-second scale, on three days of data (F1-a).

**Does not hold so far**

- The relative-value idea itself. The gap to β × BTC or β × market mostly does not close, and at one day it widens (X3-a). κ is near zero or negative for most coins.
- Price levels as magnets: high-volume zones, absorption bars, yesterday's point of control, VWAP (V-08 to V-12, V-20).
- "Squeeze, then expansion" (P-03), "H, L and C rising together is a trend" (P-02), and the session-gap and US-open rules (P-09, P-10).
- Buying a 1% drop with a fixed bracket (BT-06), and the 0.45% stop (BT-11).

**Nothing is shown to be tradable yet.** Sign patterns are worth about 0.4 bps per trade and die with a 0.5 bp offset (BT-02, BT-03). At 9 bps per side almost no cross-asset rule stays positive in both halves (BT-19). The maker results need a fill at the touch (BT-12) and do not carry out of sample (BT-14). The closest are the resting-order entries in BT-21, which are long only and include market drift.

---

## 3. Volume

No tables are stored for this section in this copy, apart from the data-kit test and the flow files.

| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| V-01 | Volume is roughly lognormal; gold is the exception | Distribution tables per asset and per year (`volume_basics`) | Your note: holds for every asset except gold. Still open: is gold the exception for more of the market, and do the distributions change by year? | ✅ | NOTE |
| V-02 | Volume is mean reverting | Chunk means, mean crossings against a sine curve, proper tests (`volume_meanrev`) | Your notes disagree. Section summary: "roughly mean reverting". Test cell: "No, but it has lots of memory" and "yes and no, long and short memory". More mean crossings than a sine curve. | 🟡 | NOTE |
| V-03 | The volume mean drifts over time | Means of 10, 100 and 1,000 chunks | Yes. It swings both ways, with a long-term downtrend. | ✅ | NOTE |
| V-04 | Low and slowing volume goes with negative returns | `volume_meanrev`, question 3 | Your note says something different: "low volume and imbalance leads to drift in the imbalance direction". | 🟡 | NOTE |
| V-05 | High volume goes with high absolute return (Q1) | Relative volume against absolute return, seasonality removed; held-out fit | Held-out rank correlation 0.42, 0.42, 0.37 for BTC, ETH, SOL (Jan to Mar 2026, p below 1e-110). Earlier run: a top-10% volume minute is 4.0 to 4.3 times as likely to be a top-10% move (FX and gold 2.8 to 2.9), and the next minute still 2.9 to 3.3 times. | ✅ | NB, PREV |
| V-06 | Low volume goes with low absolute return (Q2) | Same | Yes, weaker. A bottom-10% volume minute is 2.2 to 3.7 times as likely to be a bottom-10% move. In the lowest volume fifth a top-fifth move is 7 to 10 times less likely. | ✅ | PREV |
| V-07 | Rolling volume predicts the next window's volatility (Q3) | Correlation, and partial correlation given current volatility | Mostly a repeat of current volatility. Correlation with the next window 0.31 to 0.40 in crypto. Beyond current volatility: about +0.05 at 5 minutes, near 0 at 15, -0.1 to -0.25 from 1 hour to 1 day. Next-hour volatility is 1.39 to 1.61 times its average in the top quartile of relative volume and 0.67 to 0.77 in the bottom. | 🟡 | PREV |
| V-08 | High-volume price levels are revisited 1 to 2 hours later (Q4) | Zone around the bar, conditional on price leaving, against a volatility-matched null | Barely. Raw lift 1.2 to 1.6 shrinks to about 1.05 (BTC 28.4% against 27.1%). Nothing in FX. | 🟡 | PREV |
| V-09 | A big move on low volume returns to the pre-jump price within 1 hour (Q5, Q6b) | Top-1% jumps over 1 to 10 minutes, split by volume | No. 1 to 2 minute jumps pull back slightly whatever the volume. 10-minute jumps keep going (retrace to extend 0.74 to 0.85, z -5 to -10), and the high-volume ones continue more (4 of 6 assets). "Fade halfway, then continue" shows nothing. | ❌ | PREV |
| V-10 | A high-volume tight range is revisited within 1 to 3 days after price leaves (Q6a) | Against low-volume ranges of the same shape | Revisited less in crypto (BTC 76% against 81% within a day, z -7). No difference in FX. | 🔁 | PREV |
| V-11 | Absorption bars (very high volume, small move) mark levels that get revisited (P1) | Against impulse bars with the same volume | Not special (BTC 81% against 83%, z -4.6). | ❌ | PREV |
| V-12 | Yesterday's point of control is a magnet (P2) | Touch within a day against the mirror level; against VWAP and the mid-range | No. Lifts 0.93 to 1.08. | ❌ | PREV |
| V-13 | Fair-value-gap midpoints get filled, low-volume gaps more often (P3) | 3-candle gaps on 5-minute bars against the mirror level | The midpoint is reached slightly more than chance on 5 of 6 assets (BTC 72% against 70%). Displacement volume makes no difference. | 🟡 | PREV |
| V-14 | Busy price areas (30-day volume at price) are revisited more than quiet ones at the same distance | `volume_vap` | No result on file. | ⏳ | |
| V-15 | Moves come with recognisable volume profiles before, during and after | `volume_shapes`: moves of 1 minute to 1 day, volume around them, continuation after 60 minutes and 1 day | Your note: "rough first results: yes there are volume profiles". The path study adds that U-shaped or ramping volume inside a trend says nothing about continuation (PS-09). | 🟡 | NOTE, NB |
| V-16 | Volume is seasonal within the day and the week | `volume_signals`, question 1 | Your note: yes, by hour and by weekday. | ✅ | NOTE |
| V-17 | The volume level was properly adjusted before the tests (weekday, daylight saving) | Old against new relative volume | Your note: "already accounted for". | ✅ | NOTE |
| V-18 | sign(Σ volume × return) predicts direction | Balanced accuracy over lookback L and horizon h | In reverse. It is right 47% to 49% of the time ("anti-memory"). Plain momentum does the same and is slightly better on 5 to 15 minute lookbacks. | 🔁 | NOTE |
| V-19 | 30-day breakouts continue more when volume is stronger than on the first leg, or ramping in | `volume_signals`, question 4 | Blank in your notes. | ⏳ | |
| V-20 | When volume-weighted flow is balanced, price converges to VWAP | Touch VWAP against the mirror level | No note. Earlier run: VWAP did not act as an anchor. | ❌ | PREV |
| V-21 | Downtrend, then a high-volume jump down, then flat volume: continuation comes first more than 52% of the time (jump H1) | `jump_volume` on 1-hour, 4-hour and daily bars | Not run on real data. Only the synthetic self-test was run. | ⏳ | |
| V-22 | The same followed by high volume: reversion is more likely (jump H2) | Same | Not run. | ⏳ | |
| V-23 | The two cases differ (jump H3) | Same | Not run. | ⏳ | |
| V-24 | After a jump down, another one is more likely than usual, more so in crypto (jump Q1) | Same | Single-asset version not run. Across coins it holds (X4-b, X4-d). Earlier run: after a 2σ move over 10 minutes, another full move the same way within the hour followed 42.4% of drops and 38.5% of rises. | 🟡 | NB, PREV |
| V-25 | Net taker buying predicts the next return (FL) | Imbalance over L minutes against the next L minutes, given the same-window return | Flow files, BTC, ETH, SOL: positive at 1 minute (BTC z 13.0, ETH 5.6, SOL 2.8), nothing at 5, negative at 15 minutes (BTC z -5.4, SOL -3.0) and at 60 (BTC z -3.6). | ✅ | NB |
| V-26 | When flow and price disagree, the next move follows the flow | Same | No. At 1 minute the next move goes against the flow for BTC (z -7.6) and ETH (z -6.5). | 🔁 | NB |


| V-27 | Corrected for dollar volume, is the volume going up or down over time? | ... | .... |  ... | ... |
| V-28 | If the notional volume is decreasing over time, does the absolute return sizes decrease? | ... | ... | ... | ... |
| V-29 | a "normal distributed" volume distribution( as in increasing until the peak of the drop, decreasing after the drop returns normalise) with a drop in price "lands" where the volume drops down, the accumulation distribution indicator helps here | ... | ... | ... | ... |
| V-30 | The slope of the Accumulation distribution indicator correlates with the returns. Low ACC DIST correlates with a drop in the current price to relative value. Meaning price will reverse | ... | ... | ... | ... |
| V-31 | a range of continuously growing volumes, than 1 or 2 decreasing volume, than 1 growing is the sign of a "settled level". Meaning the prices will either increase/decrease until that decreasing volume and than reverse a bit on that next growing volume. | ... | ... | ... | ... |
| V-32 | A range say something like 10 out of 15( 20 out of 30, etc.) of "below average" volume indicates relates to a period of lower than usual absolute returns | ... | ... | ... | ... |

---

## 4. Price

No tables are stored for this section in this copy. The "primary conclusions" line in the notebook is still blank.

| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| P-01 | Chains of close-return signs predict the next close; adding open, high and low signs helps (H1) | Pattern tables trained on one half and scored on the other, 1 to 60 minute bars | Real but tiny: under 1 bp per bar out to 15 minutes. In the pattern backtest's training table, after 10 down closes the next is up 53.3% of the time (n 882); after 10 up closes, 45.3% (n 969). | 🟡 | PREV, NB |

| P-02 | A trend is 3 or more of O, H, L, C rising together; it fades when 2 or more slow (H2) We would expect this to continue for | Forward return after "k of 4 agree" and after fades | No continuation, except a two-day drift of 36 to 85 bps in crypto that looks like the common market factor. Path study: H, L, C all up is followed by up 47.9% of the time against 50.2% otherwise (PS-07). | ❌ | PREV, NB |

| P-03 | Shrinking ranges are consolidation and lead to expansion; growing ranges are diffusion (H3) | Efficiency ratio against the mirror surrogate; next range against baseline | "Contraction leads to expansion" failed in 23 of 24 cells. Quiet stays quiet (PS-01). | ❌ | PREV, NB |

| P-04 | Trend spec: slopes agree, spreads diverge, MA distance grows; the end shows as all three shrinking (TS) | Ingredients, trend anatomy, exit triggers | No result on file. | ⏳ | |

| P-05 | "Glorified Fibonacci 1": a move that stays inside X% reverts more than 51% of the time (H4) | Fade bracket against the mirror | Asymmetric. For σ-sized moves over 5 to 15 minutes, fading a rise beat its null in 115 cells and lost in none; fading a drop won in 3 and lost in 17. | 🟡 | PREV |

| P-06 | "Glorified Fibonacci 2": a move that breaches X% continues more than 51% of the time (H5) | Continuation bracket against the mirror | Modest, and stronger for drops. After a move of 2σ or more over 10 minutes, another full move the same way within the hour followed 42.4% of drops and 38.5% of rises; a full return followed 41.1% and 39.1%. | 🟡 | PREV |

| P-07 | Pullback and continuation areas: back to the pre-move level against an equal extension, by volume (H6) | Revisit against the mirror, low against high volume | Low-volume moves continue more here (retrace to extend 0.80 to 0.87, against 0.91 to 0.98 for high volume, 3σ moves at 60 minutes). That is the opposite of V-09. | 🔧 | PREV |

| P-08 | Retests of a swing high or low hold (H7) | Second and third touch, band in units of 1-hour volatility | Level-memory tests came back empty. Your "max price ± X%" version is a different test and is not built yet. | ❌ | PREV |

| P-09 | Today opens below yesterday's close and low: the high ends above the open, or above yesterday's low (H8) | New York session gaps against the mirror | Null so far. | ❌ | PREV |

| P-10 | The direction of the US open, from 5 minutes before to 15 minutes after, continues (H9) | Continuation and pullback entries | Null so far. | ❌ | PREV |

| P-11 | Chain probabilities ("the stupid Markov version") | P(next bar up) after each run of 3 to 10 closes (`price_extra`) | Not run here. See P-01 for the length-10 table. | ⏳ | |

| P-12 | OHLC bar states have different next-bar returns | State tables and transitions (`price_extra`) | Not run here. | ⏳ | |


| P-13 | A trend is 3 or more of O, H, L, C rising together for a period of 3 or more timesteps; it fades when 2 or more slow (H2) for 2 or more timesteps |  |  |  |  |
| P-14 | Discontinuations between close,low and open, say last close and low is above current open, this timestep's price will move up towards the prices it dropped from  |   |  |  |  |
| P-15 | Shrinking ranges have shrinking absolute returns |  |  |  |  |
| P-16 | expanding ranges have expanding absolute returns |  |  |  |  |
| P-17 | After a range of new All time highs(say the new all time high is being pushed upwards for a while until it turns around) and than drops by 2%, that downwards trend continues, with noise, but with clear downwards stability |  |  |  |  |
| P-18 | After new all time highs and than downwards trend continuation, we may reach a "global bottom" for a bit, from which there is a rebounce, we detect that rebounce by running continued bottom price since last all time high, than confirming with a positive 2% return. |  |  |  |  |
| P-19 | After a global bottom a "global maximum" is a repeat of the if the price continues increasing after the global bottom and is confirmed after a rebounce downwards. |  |  |  |  |
| P-20 | a move that moves up but stays inside X% with low volume reverts more than 51% of the time (H4) |  |  |  |  |
| P-21 | a move that moves up but breaches X% with high volume continues more than 51% of the time (H5) |  |  |  |  |
| P-22 | running a linear regression between lows has a predictive ability on the next few lows, if a low is outside(below) of the expected confidence area, that is indicative of a drop within 10 minutes to 1 hour |  |  |  |  |
| P-23 | running a linear regression between highs has a predictive ability on the next few high, if a high is outside(above) of the expected confidence area, that is indicative of a jump within 10 minutes to 1 hour |  |  |  |  |
| P-24 | A rolling cumulative volume * return action principle over all of the prices, with taker buy volume added on multiple horizons, say 20, 60, 1440, 7 days, can help us understand if the price has been misplaced a bit and will go up or down to match the rolling action principle indicator |  |  |  |  |
| P-25 | A 1 hour drop in prices with accumulated returns below -10% is likely to be followed by another drop in prices to a further down level below -15% within 1 to 3 days  |  |  |  |  |

---

## 5. Trend/direction study

| ID | Hypothesis | Result (event against control) | Status |
|---|---|---|---|
| TR-01 | A window of 3 to 10 pips with positive close returns is a trend, the cumulative return is in the upper 20% |  |  |  |  |
| TR-02 | Trends(the cumulative return is in the upper 20%) with varying window lenghts, 3 to 20 pips, have a short range of "Stupid markov indicator windows", like "UUUDUUUDU" |  |  |  |  |
| TR-03 | Trends(the cumulative return is in the upper 20%) has quite similar yet specific volume profiles. |  |  |  |  |
| TR-04 | Trends(the cumulative return is in the upper 10%) has lows getting higher, highs getting higher, they are outside of the confidence area of the rolling regressions for highs and lows. |  |  |  |  |
| TR-05 | Trends(the cumulative return is in the upper 10%) trends end with the last two pips having lower absolute returns than the previous 2 pips, same goes for volume |  |  |  |  |
| TR-06 | The short window volatility(3, 5, or 10) of a long term Trends(the cumulative return is in the upper 10%)(60,240, 1440) lowers relative to a longer rolling window volatility(30,60,90) before the trend fades |  |  |  |  |


## 6. Path study

`ohlcv_path_research`. BTC, ETH, DOGE and SOL pooled, 1-hour bars unless stated, scored from 2025-01-01 in weekly blocks, q adjusted within the family. Each row compares event bars with control bars. Gold and daily bars mostly had too few samples. All rows are `NB`.

| ID | Hypothesis | Result (event against control) | Status |
|---|---|---|---|
| PS-01 | After a quiet contraction, volatility stays low | Next-6-hour volatility ratio 0.48 against 0.90 (q 0.014). The "plateau" outcome follows 81% of the time against 42%. Still lower after 24 hours (0.68 against 0.98). | ✅ |
| PS-02 | Narrowing ranges with falling volume: volatility stays low | 0.75 against 0.91 over 6 hours (q 0.014). Gone after 24 hours. | ✅ |
| PS-03 | After an expansion, volatility stays high | 1.50 against 0.88 over 6 hours, 1.54 against 0.95 over 24 hours, 1.63 against 0.89 on 4-hour bars (q 0.014 each). | ✅ |

| PS-04 | Both close gaps narrowing: lower volatility next | 0.87 against 0.90 (q 0.18). | ❌ |

| PS-05 | Below the moving average: the next 6 hours are up | 51.8% against 48.0% (+3.9 pp, interval 1.4 to 6.2, q 0.014). Not above 50% after correction (q 0.22). Nothing at longer horizons or on 4-hour and daily bars. | 🟡 |
| PS-06 | Above the moving average: the next 6 hours are down | 53.0% against 48.2% (+4.8 pp, interval 2.1 to 7.4, q 0.014). Against 50%: q 0.07. Nothing at longer horizons. | 🟡 |

| PS-07 | H, L and C moving together: continuation | All up: up 47.9% against 50.2%. All down: down 47.0% against 51.2% (-4.2 pp, interval -7.2 to -1.5). It leans to reversal. | ❌ |

| PS-08 | Below the moving average inside a downtrend, or inside an uptrend: up | Not significant (q 0.37 and 0.34). | ❌ |
| PS-09 | U-shaped or ramping volume inside a trend: continuation | No effect (q 0.12 or higher in every cell). | ❌ |

| PS-10 | Volume ramps, then drops: quieter next | Volatility ratio 0.81 against 0.97 (q 0.024). No effect on direction. | ✅ |
| PS-11 | Path features forecast the next window's volatility | BTC, next 6 hours: R² 0.164 with price only, 0.222 with volume shapes, 0.231 with action, 0.254 with signatures. | ✅ |

| PS-12 | Path features forecast the next window's direction | BTC, next 6 hours, price-only model: R² about -0.001, sign accuracy 48.6%. | ❌ |


| PS-13 | close price is Below the moving average-2sigma of the average distance to the moving average, the accumulative return is going to be positive within the next 10 minutes | ... | ... |
| PS-14 | close price is above the moving average+2sigma of the average distance to the moving average, the accumulative return is going to be negative within the next 10 minutes | ... | ... |
| PS-15 | most of H, L and C moving together: continuation for a period of 5 to 30 minutes | mostly up for 5 to 30 mins.: up X% against Y%. mostly down for 5 to 30 mins: down X% against Y% |  |
| PS-16 | "path extremes" have an above 51% chance of predicting the next direction. If a pip is at the same time "above the moving averages", say at the same time above moving average 10, 20, 60 with at least 1.5 standard deviation distance from moving average on both the high, low and close. The sign indicator is "up, up, up, up, up" and the cumulative return is in the top 10% quantile. Along with the volume having been ramping up. We are more than 51% likely to have a positive return on the next pip as well. | ... | ... |
| PS-17 | "path extremes" have an above 51% chance of predicting the next direction. If a pip is at the same time "below the moving averages", say at the same time below moving average 10, 20, 60 with at least 1.5 standard deviation distance from moving average on both the high, low and close. The sign indicator is "down, down, down, down, down " and the cumulative return is in the bottom 10% quantile. Along with the volume having been ramping up. We are more than 51% likely to have a negative return on the next pip as well. | ... | ... |



---

## 7. Volatility

| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VO-01 | In some periods the highs are the more volatile series (sellers coming down to meet price), in others the lows (buyers coming up) | Open slot. `vt.volatility_features` gives the rolling volatility of O, H, L, C | Not tested. | ⏳ | |
| VO-02 | Volatility is cyclical: low to high and back | Open slot | The persistence half holds (PS-01, PS-03). Cycle length, and whether the position in the cycle says anything about the next move, are not tested. | 🟡 | NB |
| VO-03 | A random forest can call the direction of rolling volatility | Forest against the majority class, a one-line window rule and shuffled bars; 80 / 20 split in time order | No result on file. The earlier cell was interrupted. | ⏳ | |

---

## 8. Cross-asset

Leader: BTCUSDT. All rows are `NB`. Returns are scaled by each asset's own trailing volatility.

| ID | Hypothesis | Test | Result | Status |
|---|---|---|---|---|
| X1-a | Returns are correlated across assets | Correlation at 5 minutes to 1 day | With BTC: 0.52 to 0.73 for most coins at every horizon, ETH 0.82 to 0.85. Thin coins rise with the horizon (AURORA 0.07 to 0.38, CRO 0.47 to 0.62). FX about 0 to 0.16, metals 0.13 to 0.26. Mean coin-to-coin 0.56 to 0.59. | ✅ |
| X1-b | Coins are tied to BTC more in crashes than in rallies | Share of BTC's worst 5% bars that are also the coin's worst 5%, against the same for the best 5% | 1-hour bars: 47% to 58% in the lower tail (ETH 67%) against 35% to 47% in the upper tail and 28% to 42% implied by correlation alone. Lower minus upper is +10 to +15 pp (z 7 to 10), and +16 to +36 pp on daily bars. Robust on 19 of 21 coins. Nothing in FX or metals. | ✅ |

| X2-a | BTC leads the other coins at 1 minute | Slope of the coin's next bar on BTC's last bar, own bar held fixed; then one bar late | At once: every coin (z 11 to 59). One bar late: about zero for liquid coins (z -2.0 to 5.8), large only for AURORA (19.2) and CRO (24.4), whose closes are unchanged in 51% and 34% of minutes. Following BTC's sign earned 0.1 to 0.5 bps on the next minute before costs. | 🟡 |
| X2-b | Other assets lead BTC | The same in reverse | ETH leads BTC more than it follows (z -5.3). Gold, silver and GBPUSD lead BTC at 1 and 5 minutes more than they follow (z -2.2 to -6.2). | ✅ |
| X2-c | After a BTC jump (a 1-minute bar of 3σ or more) the others continue and catch up | 34,780 jumps. Catch-up = the coin's continuation minus β × BTC's own | +0.6 to +1.7 bps of catch-up in the next minute (z 7 to 16), up to about 2.6 bps over 5 to 60 minutes. Robust on 19 of 21 coins for the next 1 and 5 minutes, 17 to 18 for 15 and 60. Mostly gone for 5-minute jump bars (10 of 21). | ✅ |

| X3-a | The gap to β × BTC, or to β × market, closes (κ above 0) | 5-minute bars, β from a trailing 3-day regression, slope of the next W bars' gap on today's gap D | Mostly no: 9 robust closing, 6 robust widening, 759 not robust. Over 1 hour it closes on a few coins (LTC z 3.3, NEAR 3.9, XRP 3.4, DOGE 2.6 to 3.2) and widens on TRX (z -5.6), AURORA and ETH. Over 1 day the sign is mostly negative. | ❌ |
| X3-b | Fading a gap of 2 or more pays | Return of holding -sign(D) × (coin - β × BTC) for W bars | 1 hour: positive on 16 of 21 coins, +0.2 to +10 bps per trade before costs (NEAR 10.0, CRO 9.4, DOGE 8.2, XRP 6.6), but robust on only 2; negative on TRX, AURORA, BNB, ETH, FIL. 1 day: -9 to -156 bps on 18 of 21 coins. | 🟡 |
| X3-c | The gap closes faster when the factor is calm | Slope when BTC's volatility is below against above its median | No consistent difference. | ❌ |


| X4-a | When BTC crashes (a 1-hour bar beyond 3σ down) the others crash in the same hour | Hourly panel | 31% to 50% of the time (ETH 63%) against a usual 0.7% to 1%. | ✅ |
| X4-b | After a BTC crash, coins that did not crash follow later | Excess over the usual rate | +3.7 pp within 1 hour (robust on 19 of 21), +6.2 pp within 6 hours (16 of 21), +6.6 pp within 24 hours (13 of 21). | ✅ |
| X4-c | It spreads as a crash: down more than up | Excess for down-jumps minus excess for up-jumps | Robust on only 8 of 21 within 1 hour and 2 of 21 within 6 hours. | 🟡 |
| X4-d | A broad crash (a quarter of the coins in one hour) is followed by another | 467 broad crashes | Another one is +5.9 pp more likely within 1 hour (z 5.3), +9.5 pp within 6 hours (z 6.0), +11.0 pp within 24 hours (z 5.3). Broad crashes repeat more than narrow ones: +4.6 pp within 1 hour (z 4.0). | ✅ |
| X4-e | FX and metals jump when crypto crashes | Same hour and the next | +5.6 to +6.7 pp in the same hour (z 3.3 to 3.7), almost all of it from the second half of the sample. | 🟡 |

| X5 | Net taker buying in BTC predicts the other coins beyond both prices | Slope on the coin's next L minutes, given both returns and the coin's own flow | It predicts lower returns: about -0.009 over 15 minutes (robust on 14 of 19 coins) and -0.016 over 60 minutes (19 of 19). Nothing at 5 minutes. | 🔁 |


| F1-a | Futures lead spot | BTC order-book mids on 250 ms steps, 3 common days | Slope 0.31 at 0.25 s (z 69) and 0.26 at 1 s and 5 s. One step late: 0.07 to 0.09 (z 9 to 31). Spot to futures: 0.01 to 0.02. Worth 0.2 to 0.3 bps per step before costs. | ✅ |
| F1-b | One side closes the basis | Basis against its 15-minute average | Basis mean -4.5 bps, sd 0.71 bps, half-life about 3.8 seconds. The closing tests returned unstable slopes (hundreds to thousands, the two halves far apart). | 🔧 |
| F1-c | A jump on one side is followed on the other | Bars of 4σ or more | After a spot jump, futures continue 0.4 to 0.7σ (z 4 to 19). Several futures-jump rows blew up. | 🔧 |
| F1-d | The same on 1-minute futures bars over years | Needs futures klines | No futures klines in the data folder. | ⏳ |

| B1 | Order-book imbalance and the microprice gap predict the next mid move | 250 ms grid, next 1 to 60 seconds, BTC spot (4 days) and futures (3 days) | No robust effect. Slopes explode in one half (for example 50,088 against 0.29). | 🔧 |
| B2 | A wide spread precedes a bigger move | Same | No robust effect, same problem. | 🔧 |
| B3 | Trade flow predicts the next move | Net aggressive buying over w seconds | No robust effect, same problem. | 🔧 |

**The board.** 2,457 tests. Robust rows: X1 38; X2 and X5 443 positive and 51 negative; X3 9 positive and 6 negative; X4 73; books 26, all from F1. 29 test-and-variant combinations are robust on at least 2 assets.

**Book facts.** Median spread 0.002 bps on spot and 0.016 bps on futures. Top-level imbalance has a 1-second autocorrelation of 0.83 and 0.73.


N-11 — Microshocks arrive increasingly often before a larger move
- Hypothesis: A rising arrival rate of small price shocks predicts a new large move beyond current volatility, recent returns, volume, and recent large-event history.
- Data: Liquid assets, one-minute OHLCV.
- Features: Counts of microshocks in each of the last six 10-minute blocks; latest-10-minute count minus the earlier-50-minute count divided by five; elapsed time since the last microshock; shock magnitudes. The count slope tests acceleration rather than just elevated activity.
- Outcome: A new large-move event within the next 60 minutes. Test up and down events separately as secondary outcomes.
- Null: M0; also compare equally volatile periods with similar cumulative returns and volume but stable or falling shock arrival rates.
- Pass: Positive held-out incremental event-probability value, including in the first-event stratum. An effect confined to the aftershock stratum is useful but must be labelled accordingly.
- Next model only if justified: Signed marked Hawkes intensities, or a discrete-time analogue for minute-binned observations. Do not fabricate precise within-minute event times.
- Status: Untested.
N-12 — One-sided shocks become less reversible before directional persistence
- Hypothesis: Given the same total volatility and past return, increasingly one-sided shocks whose price impact remains after five minutes predict further movement in that direction.
- Features: RV+ = sum(r^2 for r > 0), RV- = sum(r^2 for r < 0), and A = (RV+ - RV-) / (RV+ + RV-) over the last hour. Separately measure five-minute retention after completed microshocks: sign(r_s) * log(C_(s+5)/C_(s-1)) / abs(r_s). A value near one means the initial displacement remains; zero means it has reversed at that checkpoint. Use only shocks with s+5 <= t, require a minimum count, and otherwise mark retention missing.
- Outcome: sign(A) times the next-hour return, plus directional trend probability. Test the opposite sign as a declared alternative, not a post-hoc relabelling of failure.
- Null: M0 including the past-hour return and total realised variance; assess imbalance, retention, and their interaction separately before combining them.
- Why new: It tests shock polarity and observed recovery, rather than the already weak volume-times-return signal or simple runs of candle signs.
- Literature boundary: Realised semivariance has evidence for volatility forecasting in equities [2]. That does not demonstrate this proposed directional effect in crypto.
- Status: Untested.
N-13 — Price becomes more responsive to a given amount of trading
- Hypothesis: An unusually large and increasing range for the prevailing participation level precedes larger future movements.
- Features: Fit a trailing model of log positive range on log positive quote notional, prior volatility and time of week. Use the out-of-sample range residual and its recent change. Compare against the absolute-return residual as a separately declared variant.
- Outcome: Next-hour large-move probability and range. Direction is a separate test using an explicitly signed feature; the range residual has no inherent buy/sell direction.
- Null: M0 plus raw range and volume, including their ordinary nonlinear relationship.
- Interpretation: A possible proxy for changing liquidity sensitivity. OHLCV alone cannot identify depth depletion, trader intent, or causal price impact. Exclude or flag zero-volume/zero-range observations rather than hiding them behind a tiny denominator.
- Why new: V-05 establishes co-movement of activity and price variation. This asks whether an unusual change in that relationship forecasts a later event.
- Status: Untested.
N-14 — Two-sided oscillation converts into directional displacement
- Hypothesis: After substantial back-and-forth activity, the start of more efficient directional movement predicts continuation beyond an equivalent momentum signal without the preceding oscillation.
- Features: In the earlier 45 minutes, measure high summed log high–low ranges with low net displacement. In the latest 15 minutes, measure increasing efficiency ER = abs(sum(r)) / sum(abs(r)), directional close location, and overlap between adjacent candle ranges. Set ER missing in an entirely flat window.
- Outcome: Next-hour signed return and sustained-trend probability, using the latest 15-minute direction known at t.
- Null: Match the same recent momentum, current volatility, volume, and latest efficiency but without the earlier high-oscillation condition. Test whether the sequence adds value beyond its individual ingredients.
- Why new: This is a transition from repeated excursions to retained displacement; it does not repeat the rejected claim that quiet ranges must expand.
- Timing: This is early trend confirmation. The first directional movement is already observed. OHLC bars reveal extrema and closes, not the exact within-bar sequence of excursions.
- Status: Untested.
N-15 — A sequential drift change identifies hourly trends earlier
- Hypothesis: A causally detected change in return drift, supported across short and longer windows, predicts remaining movement over the next 1–4 hours.
- Features: An online two-sided cumulative-sum detector on standardised returns; signed returns/efficiency over 15, 60 and 240 minutes; an explicit record of the first detection time. Set the detector threshold using training data and a chosen false-alert rate. Begin with one detector and a small fixed set of horizons.
- Outcome: Remaining signed return from the observable detection time, trend-label probability, and adverse excursion. Compare at completed hourly endpoints as the primary analysis, then study minute-level updates separately.
- Null: Ordinary 1-hour and 4-hour momentum, matched on signal size and volatility; assess drift detection and cross-horizon agreement separately.
- Why new: P-02/PS-07 used aligned highs/lows/closes. This asks whether a sequential change detector adds information after controlling for existing momentum.
- Timing: Never move the signal timestamp back to the low/high that was identified only afterwards. Detection delay is part of the result.
- Status: Untested.
N-16 — Patterns transfer better when time is measured by participation
- Hypothesis: A fixed pattern of price/volume behaviour predicts the next hourly outcome more consistently in equal-notional-volume bars than in equal-clock-time bars. Shortening bar completion times may add further information.
- Construction: Set each asset's notional threshold from prior complete days; for example, the prior 20-day median daily quote notional divided by 288. Freeze it for the next day. The numerical target is a starting choice, not an optimum.
- Minute-only approximation: Aggregate whole completed minute bars until the threshold is crossed. Keep overshoot and elapsed time. Do not split or reorder unknown intraminute trades. Exact volume bars require trade-level data.
- Features: Apply a prespecified price-pattern feature to the completed volume bars; include bar duration relative to normal duration for that time of week.
- Outcome: The same next 60/240 clock minutes used by the time-bar model. Comparing next five volume bars with next five minutes would confound the horizon.
- Null: Time-bar features with equivalent elapsed-history coverage and controls for recent volume/volatility; compare at the same real decision timestamps.
- Status: Untested.
N-17 — Broadening small-shock activity precedes a market event
- Hypothesis: A rising share of liquid assets experiencing small shocks predicts a target asset's next large move beyond its own state and BTC/ETH activity.
- Features: In each completed five-minute interval, calculate the fraction of eligible peers with upward and downward microshocks, change in these fractions over 30 minutes, and concentration in a few leaders versus broad participation. Exclude the target from its peer signal.
- Outcome: Target-asset jump probability and direction over the next hour; market-wide event probability as a distinct secondary outcome.
- Null: Own state, BTC and ETH returns/volatility/shock counts, and a trailing market factor. Run raw and market-residual versions; both can be useful, but they answer different questions.
- Why new: X4 tests propagation after crashes. This version explicitly tests broadening small disturbances before the next large event, separating first-event and aftershock periods.
- Data controls: Common UTC close times, source latency where known, stale-price filters, and universe membership decided from information then available. AURORA/CRO should be excluded from the initial liquid-universe test described in your log. Resample all assets jointly by time; correlated coins are not independent replications.
- Status: Untested.
N-18 — Spread widening with one-sided depth withdrawal precedes movement
- Hypothesis: Quote spread widening combined with depletion on one side of the book predicts future mid-price direction more effectively than spread widening alone.
- Features: Quoted spread in basis points and its trailing percentile, bid/ask depth changes, depth imbalance, and order-flow imbalance where event data allow. Spread alone is unsigned.
- Outcome: Mid-price movement over 1–5 minutes first; then whether it survives at 15–60 minutes and remains executable.
- Null: Spread-only and volatility/momentum baselines; compare depth effects conditional on current flow and book state.
- Evidence boundary: Order-flow imbalance and depth have a microstructure basis [3], but the cited contemporaneous relation is not evidence of this proposed forecasting edge.
- Prerequisite: Repair B1–B3's unstable estimates; verify price units, crossed books, timestamp alignment, stale quotes, and near-zero regressors. Collect substantially more independent days. The present 3–4-day book sample can support pipeline checks, not a durable alpha claim.
- Data distinction: A high–low candle range is not a quoted bid–ask spread. This test cannot be inferred from plain OHLCV.
- Status: Untested; deferred until book data are reliable.
N-19 — A normalised pattern generalises to unseen assets and later dates
- Hypothesis: The same prespecified mapping from normalised features to outcomes works on assets absent from model fitting, in a later test period.
- Test: Compare volatility units, robust seasonal z-scores, and trailing percentile ranks using the same model, feature set and temporal splits. Hold out complete assets AND later dates. The held-out asset's own past may update an unsupervised scaler; its outcomes cannot be used to tune the mapping.
- Outcomes: Incremental predictive value, calibration, and effect size across held-out assets and regimes. Report market-factor and asset-specific results separately.
- Null: Per-asset models, raw-feature pooled models, and M0. Similar-looking charts without held-out outcome improvement do not pass.
- Status: Untested.




---

## 9. Backtests

All rows are `NB`.

### Pattern maker ("the stupid pattern"), BTC

The first 60% of the history trains, the next 20% is scored, the last 20% has not been evaluated. Zero fees, zero slippage.

| ID | What was run | Result | Status |
|---|---|---|---|
| BT-01 | Baseline: length 5, up-then-down, quote 1 bp away, fill at touch | -81.6% against +78.1% for buy and hold. 7,360 trades, 56.2% wins, -2.2 bps per trade, buy markout -1.15 bps. | ❌ |
| BT-02 | Zero offset | Length 5 +60.0%, length 7 about +144% (22,553 trades, +0.42 bps per trade, daily Sharpe 2.79), length 10 +106.9%. A cost-free bound. | 🟡 |
| BT-03 | Any price improvement needed | Offsets of 0.5 to 5 bps lose 44% to 100%. Fill only when price trades 1 bp through: -93.0%. 3 bps through: -96.8%. Always quoting: -99.8% or worse. | ❌ |
| BT-04 | Fill-conditioned policy, length 10 | The one positive family with an offset: +10% to +57% at 0.5 to 2 bps (1,087 to 1,681 trades, +1.7 to +4.2 bps per trade), -2% at 5 bps. One family in a 105-row grid on the scored segment. Not checked with through fills or on the hold-out. | 🔧 |

### Liquidity gap (low-volume drop), BTC, ETH, DOGE, SOL, XAUUSD

Maker 0, taker 9 bps, 2 bps slippage on market exits. Gold: signal and exit on bid, entry on ask.

| ID | What was run | Result | Status |
|---|---|---|---|
| BT-05 | Event study: a 1% drop on low volume (lowest 10%) is a gap that refills | Such drops barely exist in the majors (BTC 0 to 1 raw cases, ETH 14). Where they do, recovery is more frequent (DOGE 77% to 85%, SOL 65% to 68%, against 58% to 63% for other drops), but the mean return over the horizon is often negative, and only a quarter to a third of 1% drops recover before a 50 bp stop. | 🟡 |
| BT-06 | Buy every 1% drop: bid 1% under the last close, target +0.5%, stop -0.5% | BTC: 779 trades, 46.3% wins, -9.5 bps per trade, -53%. ETH -86%, DOGE -100%, SOL -99.8%. With touch fills and no slippage BTC is still -7.6 bps per trade at 47.1% wins. Break-even at these exits is about 55%. | ❌ |
| BT-07 | The same, only on low-volume drops | 5 to 48 trades per asset (BTC +5.6 bps on 5, SOL +7.5 on 47, ETH -5.5 on 14, DOGE -3.2 on 48). Too few to judge. | 🔧 |
| BT-08 | Volatility-scaled distances (ATR), and a threshold search | Every ATR variant is negative (-1.6 to -11.2 bps per trade). The search over 39 candidates ended with "no candidate passed train/validation requirements". | ❌ |
| BT-09 | Smaller and larger drops (exploratory, BTC) | After 0.5% drops, low-volume recovery is 72% to 85% against 61% to 68% for other drops (15 minutes, lowest 10%: 77.8%, interval 69.4 to 84.4, n 117). Payoff mixed: -13 to +21 bps. Deeper drops recover less often: 47% to 56% after 1%, 40% to 52% after 1.5%. | 🟡 |

### Market-maker idea (your spec)

3,456 backtests per asset (36 bids × 12 sell rules × 2 stop settings × 4 fill rules) on BTC, ETH, DOGE, SOL, XAUUSD. Zero fees. The first 60% is in sample.

| ID | What was run | Result | Status |
|---|---|---|---|
| BT-10 | Bid distance under the rolling low | Median out-of-sample Sharpe is negative at every distance except 2% under the lowest (+0.12, 57.8% of setups positive). Window, and low against close, change little. | 🟡 |
| BT-11 | A 0.45% stop | With the stop: 37.6% wins, -2.06 bps per trade, out-of-sample Sharpe -0.90. Without: 87.9% wins, +40.7 bps per trade, out-of-sample Sharpe +0.09, but 75% exposure, 41% drawdown and positions left open. The stop is better in 12% of 1,656 matched pairs (median change -1.11). | ❌ |
| BT-12 | Fill rule | Touch +3.79 bps per trade, 1 bp through +2.06, close at the level -8.72, close through -10.13. | 🟡 |
| BT-13 | Target by volume state, or in units of σ | Wider targets earn more per trade (high-volume fills: +1.57 bps at 0.3%, +10.22 at 1%; σ targets: +0.31 at 1σ, +7.06 at 3σ), but the median out-of-sample Sharpe stays negative (-0.15 to -0.28). | 🟡 |
| BT-14 | The best in-sample setups carry forward | No. On BTC, in-sample Sharpe 1.3 to 1.8 becomes -0.75 to -7.2. 2021 carries the result; 2024 to 2026 are negative for the best three. A 9 bps fee on the stop turns +0.30 into -1.85 bps per trade. Top tenth, in sample to out of sample: BTC 0.88 to -0.57, ETH 0.65 to -0.21, DOGE 0.69 to -0.22, SOL 1.58 to +0.25. | ❌ |
| BT-15 | Gold | Holds out of sample (top tenth 1.80 to 1.12; best setup 2.40 to 1.80, +2.78 bps per trade). The gold bars are mid prices, so no spread is charged. | 🔧 |
| BT-16 | Regimes of the best BTC setup | Low-volume fills +1.90 bps (80.8% wins) against +0.15. Wild volatility +1.35 against calm -0.24. Europe 8 to 13 UTC +1.11. After a leader drop +0.73 against +0.12. Weekend +0.69 against +0.16. Whole sample, setup chosen in sample: leads, not results. | 🔧 |

### Cross-asset rules

178 rules per asset (follow 104, reverse 36, relative value 24, flow 8, contagion 6). Costs 0 to 9 bps per side. Entered at the close of the signal bar and one bar late.

| ID | What was run | Result | Status |
|---|---|---|---|
| BT-17 | Follow the leader after a BTC jump | The best rules bear 1 to 3.5 bps per side when entered at once. One bar late the family keeps about 10% (median 0.51 to 0.04 bps). | ❌ |
| BT-18 | Fade after contagion; fade a 1-day relative-value gap | Highest break-even costs: contagion fade on 60-minute bars, hold 24 (13.6 bps median, -22 to +35 across coins, 0.2 trades a day), and relative value W=288 fade (11.2 bps, -55 to +53, positive in both halves on 6 to 7 of 20 coins). | 🟡 |
| BT-19 | What survives 9 bps per side | Pairs positive in both halves: follow 10 of 1,300 at once and 0 one bar late; relative value 13 of 288 at once and 10 one bar late; contagion 5 of 150; flow 0 of 208; reverse 0 of 450. About half of the best rows are AURORA (stale prices). | ❌ |
| BT-20 | Portfolio of the relative-value fade (equal weight, one bar late) | Both sides: 5.6% a year at zero cost, -3.6% at 9 bps. Long only: 13.6% falling to 8.7% (Sharpe 0.73 to 0.51), which includes market drift. | 🟡 |
| BT-21 | Resting-order entries, long side | Relative-value fade W=288: +91 to +95 bps per fill, but only 2.2% of signals fill (16 of 16 coins positive, Sharpe about 0.65). Contagion fade: +25 to +28 bps at 71% fills. Follow 5-minute jumps, hold 60: +10 to +12 bps at about 64% fills, with drawdowns of about 80% to 85%. Long only, so drift is included. | 🟡 |

---

## 9. Contradictions and checks

Settle these before building on the results.

1. **Is volume mean reverting?** The section summary says "roughly mean reverting"; the test cell says "No, but it has lots of memory" (V-02). The long-memory estimate in your notes (fractional d, Hurst) is the test that settles the wording.
2. **Volume flips sign between two modules.** V-09: high-volume 10-minute jumps continue more (retrace to extend 0.63 to 0.81, against 0.83 to 0.95 for low volume). P-07: low-volume moves continue more (0.80 to 0.87 against 0.91 to 0.98). The event definitions differ. Resolve it before volume becomes a state feature.
3. **Relative value at one day.** X3-a says the hedged gap widens at one day, yet the W=288 fade rule shows a positive median break-even (BT-18). That rule trades the coin unhedged, its range across coins is -55 to +53 bps, and the hedged *follow* version is positive too (3 bps). Compare hedged and unhedged side by side before reading it as relative value.
4. **Unstable order-book estimates.** F1-b, F1-c and B1 to B3 have slopes thousands of times larger in one half than in the other. Those rows can't be read until that is fixed.
5. **AURORA and CRO.** Their closes are unchanged in 51% and 34% of minutes. Their lead-lag scores, and about half of the best 9 bps rows, are probably stale prices.
6. **Gold maker results** (BT-15) are on mid prices without a spread.
7. **EURUSD is missing** from this run. The earlier results used it as one of the two non-crypto checks.
8. **Four coins are one factor.** ETH, DOGE and SOL correlate 0.55 to 0.84 with BTC on 1-hour returns, so four agreeing results are closer to one. The path study and most `PREV` rows rest on these four.
9. **Survivors only.** The coin list has no dead coins (LUNA, FTT), so tail and floor estimates are too kind.
10. **Not causal yet.** The earlier event studies used full-sample volume terciles, quantiles and hour-of-week profiles. That is fine for discovery and a leak once they become model inputs.
11. **Hold-outs.** The pattern maker's last 20% is unevaluated. The market-maker and cross-asset grids have now seen the whole sample.
12. **Many tests.** 2,457 on the cross-asset board alone. Trust what repeats across assets and across both halves.

---

## 11. Backlog

### A. Ideas written in the notebook, not tested yet

From the first cell:

- [ ] Take out the top 60% of volume. How much does price move in the windows that are left?
- [ ] Are the windows with the flattest volume also the windows with the flattest returns?
- [ ] Participation on sudden corrections: is it nominal or dollar value?
- [ ] Does a higher mean volume for a period mean higher absolute returns once the top 5% or 10% of outliers are removed?
- [ ] Large volume on a drop or jump as participation when price is off its relative value; low volume at the "correct" price.
- [ ] Large volume into a rebound.
- [ ] What happens between "significantly smaller volumes"?
- [ ] Do prices stay inside the area between big negative-volume and big positive-volume bars?
- [ ] "En ulykke kommer sjældent alene": after a drop, how long until the next one, with drops defined as volume above 2σ? (Across coins: X4. Per coin: V-21 to V-24.)

Volume:

- [ ] Big volume followed by continued big volume, against big volume fading into smaller (the peak profile): does the second mean a drop and a recovery of less than 100%?
- [ ] Is gold the exception for more of the market? Do the distributions differ by year?
- [ ] Long and short memory: fractional d (GPH or local Whittle), DFA Hurst exponent, autocorrelation at 1 hour, 1 day and 1 week.
- [ ] One model with total volume share at the level, time and volume combined.
- [ ] At a high-volume area, does price bounce or pass through? A touch says price got there; a reaction says the level mattered.
- [ ] Volume "before but leading up to" a move, against volume "during" it.
- [ ] "Proper mean reversion of low volume / high volume to price before."

Price:

- [ ] Retests inside "max price ± X%" with a fixed band (P-08 uses volatility bands).
- [ ] Session-gap and US-open tests on index data (the optional US500 and US100 download was not run).

Volatility:

- [ ] High-more-volatile against low-more-volatile periods, and what the highs and lows do next (VO-01).
- [ ] Length of the volatility cycle, and whether the position in it says anything about the next move (VO-02).

Backtests and data:

- [ ] Your own market-maker slot: more entry, exit and stop features.
- [ ] Your own cross-asset rule.

### B. Hypotheses the results point to

These are suggestions drawn from the tables above, not results. The first three can be tested by joining regime-engine states to the trades.

| ID | Hypothesis | Comes from | First test |
|---|---|---|---|
| N-01 | The 1% dip bracket wins more often when the fill bar is on low volume | BT-16 (+1.90 against +0.15 bps), BT-09 | Win rate and bps by `participation` state, scored out of sample. Baseline to beat: 46% to 47% wins against about 55% needed (BT-06). |
| N-02 | The dip bracket does better in high-volatility states than in calm ones | BT-16 (wild +1.35, calm -0.24) | The same split by `vol_level`. |
| N-03 | Dip-buying in the 1 to 6 hours after a BTC crash or a broad crash loses more than usual | X4-b, X4-d | Split by hours since the last 3σ hourly down-jump in BTC, and by `risk` state. |
| N-04 | Rises fade and drops do not: a sell-the-rise bracket beats its mirror, a buy-the-drop bracket does not | P-05, P-06 | Re-run the fade and continuation brackets on the 26 assets, both directions under one rule. |
| N-05 | Heavy BTC taker buying over the last 15 to 60 minutes is followed by lower returns in the other coins, by enough for a maker to use | X5, V-25 | Use it as a filter on bids; score with touch and through fills. |
| N-06 | After a BTC 3σ minute, a resting order in the other coins captures the catch-up | X2-c, BT-21 | Hedged instead of long only, one bar late, AURORA and CRO left out. |
| N-07 | Targets and stops in units of current volatility beat fixed percentages | PS-01, PS-03, BT-13 | The same bracket with distances scaled by the volatility state, compared out of sample. |
| N-08 | The 1-hour gap to β × BTC closes on a fixed set of coins (LTC, NEAR, XRP, DOGE) and not on the rest | X3-a | Freeze the list, then test on data after 2026-10-24 with a maker entry. |
| N-09 | High volume at the start of a move means continuation; high volume late in a move means exhaustion | Check 2 in section 9 | One event definition, with volume measured before, during and after the move. |
| N-10 | Order-book imbalance predicts the next 1 to 10 seconds once the regression is stable | B1 to B3 | Rank or winsorised regression, checked day by day. |

### C. Re-runs and fixes

- [ ] Run the 13 suites on the 26 assets and save their scoreboards, so this log can cite stored numbers: `volume_basics`, `volume_meanrev`, `volume_memory`, `volume_vap`, `volume_shapes`, `volume_signals`, `jump_volume`, `return_volume`, `taker_flow`, `price_structure`, `price_events`, `price_extra`, `volatility`.
- [ ] Fix the EURUSD loader (timestamp column).
- [ ] Run the mirror-surrogate null check on the 26-asset run.
- [ ] Evaluate the pattern maker's last 20% once, with the config frozen first.
- [ ] Add 1-minute futures klines for F1-d, and ETH and SOL order books for B1 to B3.
- [ ] Fix the unstable slopes in F1-b, F1-c and B1 to B3.

---

## 11. Template for a new hypothesis

Copy this block, fill it in before running anything, and add a row to the right table when the result is in.

```markdown
### N-xx: <short name>

- **Hypothesis:** <one sentence that can turn out wrong>
- **Why:** <the result or idea it comes from>
- **Data:** <assets, period, bar size>
- **Event or state:** <what has to be true at time t, using only data up to t>
- **Outcome:** <what is measured, and over which horizon>
- **Null:** <control bars, mirror surrogate, or one bar late>
- **Pass rule, set before running:** <for example q at most 0.05 in its family, same sign in both halves, on at least N assets, above X bps after costs>
- **Result:**
- **Status:** ⏳
- **Date:**
```

Row format for the tables:

```markdown
| N-xx | hypothesis | test | result with the key numbers | status | source |
```

Before a row gets a ✅:

- [ ] Same sign in both halves, and q at most 0.05 inside its family
- [ ] Repeats on assets outside BTC, ETH, DOGE and SOL
- [ ] Still there one bar late, with AURORA and CRO left out
- [ ] Scored at 0 and at 9 bps per side, with touch and with through fills
- [ ] Every input is known at time t: no full-sample quantiles
