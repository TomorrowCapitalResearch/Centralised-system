Premises:
1. the trading strategy must survive trading fees (0.09% for taker, 0% for maker). This means we are more incline to do maker strategies.\
exchange is maker heavy.



Passive dislocation capture. This is LiqGapMR’s idea with a resting entry instead of a market buy. Keep post-only bids a set distance below a reference price (your Binance feed, or the index price the ticker returns) and re-centre them with replace. Fills happen only when Revolut X trades at a discount to the wider market, with no fee and no reaction race. The main risk is a market-wide drop filling every bid at once, so cap total exposure; this is also where a regime gate would pay for itself. 
revolut
Two-sided quoting on wide-spread pairs. Quote both sides and shift quotes down as inventory builds, with inventory running from zero to a cap. A taker paying 0.09% only profits from hitting a quote that is stale by more than that, which gives a slow maker some cover. It lives or dies on captured spread minus markout, per pair.
Slow long/flat allocation. Trend and regime positioning over hours to days across your top-30 universe, rebalanced with resting orders. Moves are measured in percent, so fees stop mattering; the hard parts are statistical power and that every position is the same BTC-beta bet.

Candidate	Initial rule	Why test it
Slow trend, long/cash	On completed hourly bars, hold a long target when SMA(16) exceeds SMA(64) and price exceeds SMA(64); otherwise target cash.	A simple benchmark with scope for less turnover.
Passive reference reversion	Rest a bid below a fresh, currency-matched external reference adjusted for normal venue differences; sell filled inventory toward the reference, with predefined risk/time exits.	Tests whether Revolut X’s maker pricing can support an actual execution edge.

| Family | description | Supplied evidence | Decision |
|---|---|---|---|
| BTC → altcoin minute lead–lag | x | x | x |
| lead–lag | leader (EUR/USD for FX, S&P 500 for indices, US 10Y for bonds) makes a ≥ 3σ minute the follower has not matched by 2σ of its beta-implied move; taker, 15 min | 15-minute follower return above null and net > 0 after costs |
| Thin-volume drop reversion | x | x | x |
| Generic sigma-band fades | x | x | x |
| ETH wick ladder | x | x | x |
| wick ladder | one resting bid m × 1-minute σ below the last close (m = 8, 12, 16), target the pre-wick close, 1 h or 4 h limit, no bid in a minute after the leader fell ≥ 3σ | discovery pick net > 0 with t > 2 and z > 2 vs null; same sign in hold-out; positive in most of the grid |
| Acceleration/participation continuation | x | x | x |
| Hourly trend | x | x | x |
| trend | daily MA / time-series momentum (28, 56, 112 days), optional efficiency ratio > 0.2, inverse-vol weights, long-only and long/short | Sharpe above buy & hold in both periods, a broad block of lookbacks |
| cross-sectional momentum | top 3 by 28/56-day return, raw and risk-adjusted, optional efficiency filter | beats equal-weight buy & hold in both periods |
| Structure-based routing | x | x | x |
| Old Bitstamp liquidity provision | x | x | x |
| Thin-volume drops revert | x | x | x |
| High-volume drops revert | x | x | x |
| heavy-participation bounce | ≥ 3 or 4 σ 5-minute drop, participation in the top third, maker bid at the event close, target the pre-move price, 4 h | 60-minute return and trade net above null (z > 2) in both periods |
| Pushes into a level with rising or persistent volume | x | x | x |
| σ-band fades ("2 sigma") | buying 2–3 σ below the rolling norm and selling above it pays.|  x | x |
| ATH → global min → local max → local min | map the all-time high, the minimum since it (global min), the maximum since that (local max) and the minimum since that (local min, merging into the global min if broken); inside these periods behaviour repeats, and σ-band / stability entries work better once the slope has stopped falling. |  x | x |



## O — overlays: "regime detection, stops and risk rules raise profits and Sharpe"

*Claim (Mark):* adding regime filters, stop-losses and systematic risk rules raises return and Sharpe.

*Test:* each overlay alone on four base strategies (1-hour σ-band fade long and short = mean reversion; 50-bar
Donchian breakout long and short = trend), all markets, one position per market, 10-slot book:
stops `sigma:1/2/3`, `trail:2/3`, `be:2`; gates `trend_up/down`, `er_low/high`, `vr_low/high`, `calm`, `not_wild`;
sizing `inv_vol:50`; book rules (kill switch 3 losses / 24 h, max 2 new trades an hour, −2 % daily stop, −10 %
drawdown brake). Positions: 10 % portfolio volatility targeting on every daily rule. Also: the ladder with a
kill switch, a 1–2 × depth stop and three gates; S1 trades with and without the 1–2σ stop.

*Statistic:* the paired daily P&L difference against the plain version on the same days (bp/day, t), plus
Sharpe and max drawdown. *Support:* t_diff > 2 in discovery and the same sign in hold-out, on both sides.

*Priors:* stops add value only when returns are positively autocorrelated (trend) and subtract it under a random
walk or mean reversion (Kaminski & Lo 2014). Volatility targeting raises Sharpe for assets whose volatility
clusters and is negatively related to returns (equities, credit) and mainly cuts tail risk elsewhere (Moreira &
Muir 2017; Harvey et al. 2018). Edgelab: an ML risk filter scored 3.30 Sharpe in discovery and 0.13 out of sample.

## C — corrected sigma

*Claim:* the S1/S3 "k sigma" thresholds mis-measure risk. Four corrections: time-of-week volatility profile
(`seasonal`), the σ of n-bar returns themselves (`direct`), bipower variation (`robust`), and quantile calibration
(threshold at the empirical quantile with the Gaussian tail probability of k). Bands: EMA distance in random-walk
units (`ema_rw`) and a rolling regression channel (`channel`), each crossed with regime gates.

*Statistics:* events per market-year, share of events in the busiest 4 hours (1/6 = evenly spread), the k = 3
rarity against the Gaussian 0.13 %, and the S1/S3 statistics re-run with each definition.

*Support:* a definition is "better" if it spreads events evenly over the clock and gives k its nominal rarity;
it only *helps a strategy* if the strategy's hold-out numbers improve, not just the event count.


## Verdicts

| | Claim | FX / indices / commodities / bonds, 2005–20 | Deciding number |
|---|---|---|---|
| S1 | Thin-volume drops revert within 6 h | **Rejected** (as on crypto) | touch rates equal the sign-flip null in every bucket; hold-out 0 of 48 trade cells > 0 |
| S2 | Pushes into a level on rising/persistent volume predict the 4 h move | **Rejected** | rising/persistent minus fading: −0.3 bp in both periods; 9 and 13 of 32 cells > 0 |
| S3 | Buy 2–3σ below the rolling norm, sell above | **Rejected** | 8 of 72 cells > 0 in each period, none t > 2; the pick's book Sharpe 0.03 → 0.03 |
| S4 | ATH → global min → local max → local min phases | **Rejected** | 7 of 120 cells \|z\| > 2 (chance ≈ 6), none repeats; phase trades lose in the hold-out |
| A1 | Wick ladder | **Rejected** off crypto | all 6 cells lose in both periods (−5.5 to −9.9 bp per fill); the pick equals its null (z 0.4, −0.3) |
| A2 | Heavy-participation bounce | **Rejected** off crypto | discovery +1.2 bp at 60 min (z 3.5) → hold-out −0.4 bp; every trade cell loses |
| A3 | Lead–lag (S&P → indices, EUR/USD → FX, US 10Y → bonds) | **Real, decaying, below costs** | indices +6.4 bp (z 5.2) → +2.0 bp (z 3.7) at 15 min; as a taker trade +1.3 → −1.7 bp |
| A4 | Daily trend + efficiency filter (+ vol targeting) | **Rejected for 2013–20** | long/short Sharpe −0.10 to +0.53 → −0.22 to −0.55 at every lookback |
| A5 | Cross-sectional momentum | **Not supported** | 28 / 56 days 0.57 / 0.69 → −0.23 / −0.17; one risk-adjusted cell held (0.60 → 0.49), its neighbours reversed |
| O | Stops | **Help only where moves persist** | S1 intraday: better in 31 of 32 hold-out cells, still negative; hourly σ-band and Donchian: \|t\| ≤ 0.6 |
| O | Regime gates | **None passes** | best hold-out paired difference t 1.6, mostly by trading less |
| O | Sizing, book rules, vol targeting | **Lower drawdown, same Sharpe** | inverse-vol halves the Donchian drawdown; vol targeting over 310 pairs: Sharpe 0.057 → 0.058 |
| C | Corrected σ | **Better measurement, no edge** | at 3σ a k·σ cut finds 4–7× more drops than the true tail; seasonal σ evens the hours; hold-out \|z\| ≤ 1.8 |
| BTC | Ladder on Bitstamp 2012–16 (never used by edgelab) | **Profitable, mostly spread capture** | +151 bp per fill, but sign-flipped copies earn +111 bp (z 2.2) |





## S2: pushes into a level on rising or persistent volume (4-hour return in the push direction)

| | discovery 2005–12 | hold-out 2013–20 |
|---|---|---|
| rising/persistent minus fading volume, 32 cells (level width, touches, break/pattern entry) | 9 of 32 > 0, mean −0.3 bp, none t > 2 | 13 of 32 > 0, mean −0.3 bp, 2 with t < −2 |
| rising/persistent pushes vs sign-flip null (8 cells per row) | z from −2.4 to +1.8, hit rate 0.46–0.52 | z from −2.6 to +1.0, hit rate 0.46–0.51 |

* Volume behaviour on the pushes makes no difference. Pushes with rising or persistent volume are followed by
  the same 4-hour return as pushes on fading volume, in both periods and in every asset class.
* The one stable pattern is unrelated to volume: an FX break above a level gives back about 1 bp over 4 hours in
  both periods (t −2.8 to −4.4, both volume groups). That is smaller than an FX round trip (1.5–2.5 bp).
  Commodity up-pushes looked good in discovery (+3.0 bp, t 2.6–2.8) and reversed in the hold-out (−0.9 to −1.2 bp).

## S3: σ-band fades ("2 sigma")

| | discovery | hold-out |
|---|---|---|
| trade cells net > 0 after costs (72: 15 min / 1 h × long / short × w × k × exit) | 8, none t > 2 | 8, none t > 2 |
| 15-minute cells (36) | 0 > 0 | 2 > 0 |
| discovery pick (1 h, Bollinger w 100, k 3, long, exit at the mean) | +0.8 bp on 182 trades, null −3.9 (z 0.3); book Sharpe 0.03 | +0.9 bp on 151 trades, null −2.4 (z 0.4); Sharpe 0.03 |
| corrected norms (Bollinger, channel, EMA random-walk band) × 6 regime gates × long/short, 1 h, k 2 (36 cells) | 35 of 36 < 0 | 35 of 36 < 0 (the survivor changed: +2.4 bp, t 0.7) |

Commodities lose most (up to −40 bp per trade in the hold-out). FX, indices and bonds mostly lose 1–4 bp.
Fading a 2–3σ stretch lost money on these markets in both periods, whichever way the norm and σ were measured.

## S4: ATH → global min → local max → local min phases

* **Phase returns:** 7 of 120 phase × anchor × horizon cells had |z| > 2 (chance ≈ 6). In discovery, falling
  phases were followed by +2.9 to +4.6 bp (z 2.1–2.7) and rebounds by −4.4 to −5.0 bp (z −3.5 to −3.9). In the
  hold-out the same cells gave −0.4 to +1.0 bp and −0.1 to −0.8 bp (|z| < 1).
* **Trades:** buying the dip while still falling, or buying the confirmed higher low, lost in every
  anchor × exit cell of the hold-out (−0.6 to −45 bp per trade). The one exception was the higher low under the
  ATH anchor with a 3-day hold (+2.6 bp, t 0.4).

## A1: wick ladder (edgelab's crypto pick) on FX, indices, commodities, bonds

| | discovery | hold-out |
|---|---|---|
| all 6 cells (m 8 / 12 / 16σ × hold 1 h / 4 h), net per fill | −5.5 to −7.7 bp | −6.6 to −9.9 bp |
| 10-slot book Sharpe | −0.8 to −3.3 | −1.8 to −4.7 |
| discovery pick (m 16, 4 h) vs sign-flip null | −6.3 vs −7.5 bp (z 0.4) | −9.9 vs −9.2 bp (z −0.3) |
| m 12, 4 h with one change at a time: seasonal σ, robust σ, kill switch, 1–2σ stop, calm / not-wild / up-trend gate | none positive (best −0.5 bp, seasonal σ) | none positive (best −4.9 bp, 1σ stop; base −8.2) |

Win rates are 50–69 %, but the losers are far larger than the winners. On mid-quote markets with constant
spreads, a resting bid below a wick earns nothing beyond the null. Bitstamp is different (below), where the
"wick" is mostly the spread.

## A2: heavy-participation bounce

| ≥ 3σ 5-minute drops, top third of participation | discovery | hold-out |
|---|---|---|
| 60-minute return vs null | +1.2 vs +0.1 bp (z 3.5) | −0.4 vs −0.1 bp (z −1.3) |
| by class, 60 min | commodities +2.7 (t 4.1), indices +1.0, bonds +0.6, FX +0.4 | indices −1.1 (t −2.5), others ≈ 0 |
| maker trade (bid at the event close, target the pre-move price, 4 h), net per fill | −3.4 bp (null −3.7) | −4.7 bp (null −4.2) |
| 10-slot book | Sharpe −2.5, max DD −82 % | −3.7, −85 % |

The crypto pattern showed up in 2005–12, small but beyond the null, and was gone in 2013–20, when index drops kept
falling instead. Even in discovery the bounce (about 1 bp) was smaller than what a resting bid loses to adverse
selection: the bid only fills when price keeps falling, and the maker version lost the same on the sign-flipped
copies as on the real data. Every trade cell loses in both periods.

## A3: lead–lag (leader makes a 3σ minute the follower hasn't matched)

| follower's signed return after 15 min | discovery | hold-out |
|---|---|---|
| indices (S&P 500 leads) | +6.4 bp (z 5.2 vs null) | +2.0 bp (z 3.7) |
| FX (EUR/USD leads) | +0.7 bp (z 3.4) | +0.3 bp (z 1.7) |
| bonds (US 10Y leads) | +1.3 bp (z 3.5) | +0.3 bp (z 0.7) |
| as a taker trade, net per trade | indices +1.3 bp (t 1.2); FX −1.5; bonds −1.7 | indices −1.7 bp (t −3.6); FX −1.8; bonds −2.5 |

The effect is real: the follower does catch up, beyond the null, in both periods for indices. But it shrank by
about two thirds and is now smaller than the cost of crossing the spread. It is usable only as a filter on
trades you were going to make anyway, or with execution well inside the placeholder spread.

## A4 / A5 and vol targeting: daily trend, cross-sectional momentum, buy and hold (all 25 markets)

| Sharpe, discovery → hold-out | raw | with 10 % vol targeting |
|---|---|---|
| buy and hold, equal weight | 0.54 → 0.13 | 0.67 → 0.13 |
| trend, long/short (MA 28 / 56 / 112 days) | 0.05 / 0.16 / 0.49 → −0.50 / −0.44 / −0.38 | 0.08 / 0.19 / 0.50 → −0.52 / −0.42 / −0.37 |
| trend, long/short, efficiency filter er > 0.2 (MA 112) | 0.28 → −0.33 | 0.29 → −0.30 |
| time-series momentum, long/short (28 / 56 / 112 days) | 0.27 / 0.27 / 0.53 → −0.37 / −0.26 / −0.28 | similar |
| cross-sectional momentum (28 / 56 days) | 0.57 / 0.69 → −0.23 / −0.17 | 0.60 / 0.59 → −0.25 / −0.02 |
| cross-sectional momentum, risk-adjusted (28 / 56 days) | 0.70 / 0.60 → −0.48 / **+0.49** | 0.63 / 0.64 → −0.41 / +0.52 |

* Daily trend made modest money in 2005–12 (long/short Sharpe 0.05–0.53) and lost in 2013–20 on these markets
  at every lookback. The efficiency filter didn't rescue it.
* One cross-sectional momentum cell held up (risk-adjusted, 56 days: 0.60 → 0.49). Its 28-day neighbour
  reversed (0.70 → −0.48), and so did its filtered version (0.75 → −0.13). One surviving cell out of a grid is
  not a result.
* **Vol targeting changed Sharpe by nothing on average:** over 310 strategy × period pairs, mean Sharpe went
  from 0.057 to 0.058. It made drawdowns slightly smaller in volatility units (−2.85 → −2.78 annual vols).
  Raw drawdowns got *bigger*, because low-volatility strategies were levered up to 10 %. This matches Harvey
  et al. (2018): vol targeting mainly evens out risk, and Sharpe gains show up only in some equity and credit
  portfolios.

## Overlays: stops, regime gates, sizing, book rules

Each overlay was added to one plain strategy at a time and compared on paired daily P&L (bp/day, t).
Strategies: hourly σ-band long and short (mean reversion), Donchian long and short (trend).

| overlay | effect in the hold-out |
|---|---|
| fixed stops 1 / 2 / 3σ, trailing 2 / 3σ, break-even at 2σ | paired difference within ±0.06 bp/day, \|t\| ≤ 0.6 on both families |
| S1's intraday drops (15–60 min), 1–2σ stop | better than no stop in 31 of 32 cells (−3.5 vs −4.4 bp per trade); still negative |
| gates: calm, not-wild, trend up / down, efficiency high / low, variance ratio high / low | best t 1.6 (variance-ratio-high on mean reversion), which mostly trades less |
| inverse-volatility sizing | Donchian max drawdown −9 % → −4 %, Sharpe unchanged |
| book rules: daily stop −2 %, drawdown brake, 3-loss kill switch, max 2 new trades per hour | never triggered, or no change (identical rows) |

**Stops add value only when moves persist.** That is the theory (Kaminski & Lo 2014: a stop helps when returns
are serially correlated, like momentum, and hurts when they are random or mean-reverting). It is also what the
data show. On S1's intraday drops, which kept going for a while, a stop cut losses in 31 of 32 cells. On hourly
mean reversion and trend it changed nothing. On crypto S1, where drops did revert at random-walk rates, the
stop made the book worse (Sharpe −2.3 → −5.9). A stop or gate cannot create an edge that the entry doesn't
have. It can shrink losses on the trades that were wrong for a persistent reason.

## C: corrected σ

| | finding |
|---|---|
| how many "3σ" 15-minute drops each definition finds | k·σ thresholds find 4–7× more than the matching tail quantile (fat tails: k·σ is not k-sigma rare); at 2σ, 0.9–1.3× |
| clustering by hour (share in the busiest 4 hours; 0.17 = even) | 0.40–0.59 with plain, robust or direct σ; **0.18–0.29 with seasonal (time-of-day) σ** |
| 6 h excess return vs null, hold-out | no definition beyond \|z\| 1.8 (q ≥ 0.5) |
| touch-back rate vs null, hold-out | seasonal-σ drops touch back 1–2.5 points more often than the null (z 8.4–8.6), with ~0 excess return: quiet-hour quote noise, not money |
| ladder with seasonal σ | best variant in discovery (−0.5 bp per fill), −6.6 bp in the hold-out |

The corrected σ does what it should: "2σ" now means the same rarity at every hour, and events stop piling up in
the open and close. It measures moves better. It adds no edge to S1, S3 or the ladder, because these didn't fail
on mismeasurement. They failed because the moves after them are random-walk moves.

## Bitstamp BTC/USD 2012–16: the crypto alphas on data edgelab never used

| wick ladder, m 12, 4 h (edgelab's pick), Revolut X costs | fresh 2012–16 | seen Jan 2017–Jan 2018 |
|---|---|---|
| fills / net per fill / t | 806 / **+151 bp** / 10.9 | 59 / +73 bp / 3.4 |
| sign-flipped copies, net per fill | +111 bp (z 2.2) | −2 bp (z 1.5) |
| win rate / worst fill | 77 % / −13.8 % | 85 % / −5.1 % |
| 10-slot book Sharpe / max DD | 4.18 / −2 % | 3.43 / −1 % |
| years positive | 5 of 5 (+94 to +297 bp per fill) | |

All six (m, hold) cells are positive in both periods. But three-quarters of the 2012–16 profit also appears on
the sign-flipped copies, which keep every bar's shape and randomise only direction: on a thin, wide-spread book
most "wicks" are the bounce between a distant bid and ask, and a resting bid inside that gap earns the spread
whichever way the market is heading. The part that is about direction (real minus null, ~40 bp per fill, z 2.2)
is real but much smaller than the headline. The ladder is a liquidity-provision trade; its profit depends on how
thin the venue's book is, which is the reason to measure Revolut X fills directly.

**S4 on BTC:** the phase map *does* separate returns here (13 of 60 cells |z| > 2 in 2012–16; chance ≈ 3), but in
the opposite direction to buying higher lows: bars near the anchor high were followed by +36 to +150 bp excess over
24 h (z 2.2–2.3), falling phases by −34 bp (7-day anchor, z −2.1), and the higher-low bounce by −22 to −31 bp
(z −2.3 for the 90-day and all-time-high anchors). BTC in 2012–17 rewarded staying with strength, as edgelab's
trend results also found. **S3 on BTC:** longs average −1.6 bp per trade in 2012–16 (20 of 36 cells > 0), shorts
lose in every cell; no edge beyond drift.

## Caveats

* **Mid quotes and tick volume.** Oanda bars are mid prices; a resting order is filled only after price trades half
  a (constant, placeholder) spread through it. Real spreads widen at the daily roll, at news and in thin hours,
  exactly when wicks happen, so any limit-order result on these data is an upper bound. Volume is a count of quote
  updates, not traded size.
* **CFDs, not futures or cash.** Index, commodity and bond CFDs carry financing (overnight funding) that is not
  charged here; it matters for the daily trend and buy-and-hold rows (roughly the short rate on the notional).
* **Costs are placeholders.** Replace them with your broker's quotes (`siglab.instruments.register`).
* **Survivorship and selection.** The 25 markets are the ones the FutureSharks repository happened to collect;
  no delisted contracts, no single stocks.
* **Multiple testing.** Hundreds of cells per study; read the q-values and neighbourhoods, not single cells.
  Hold-out was read once per frozen pick.
* **Fresh data.** June 2020 onward has not been touched. It needs `fetch_dukascopy.py` on your machine (the
  download sites are blocked from my sandbox).



# Cascading extrema: first results (31 Oct 2026)

What was tested: the ATH → global min → global max → local min → local max map from your charts, made causal
(`backtest/extrema.py`). It was used three ways: as a regime detector (up / flat / down segments between structure
breaks, falling legs, near ATH), as a bounce-level study (do new lows step down by a regular %, and does price
bounce at the projected next level?), and as strategies (`extrema_bounce`, `segment_band`), alone and routed by
structure state. Reproduce everything with `notebooks/05_cascading_extrema.ipynb`. The executed copy on your
BTC/ETH data is `notebooks/executed/05_cascading_extrema_your_btc_eth.ipynb`.


## 1. Do new global lows step down by a regular %? Mostly no

`step` = confirmed low ÷ previous confirmed low in the same cycle − 1. `dispersion` = IQR ÷ median of the step
sizes (lower = more regular). `bounce x%` = share of legs that, on first reaching x % below the broken low,
rebounded theta before falling another theta. A random walk gives 0.5. Null = 30 sign-flipped copies
(same volatility, no direction). `z` = (real − null) / null sd. Full samples:

| | lows | median step (null) z | dispersion (null) z | bounce 3 % (null) n | bounce 5 % (null) n | bounce 8 % (null) n |
|---|---:|---|---|---|---|---|
| Bitstamp BTC 2012–18 | 49 | −13.1 % (−7.2 %) −2.7 | 0.73 (1.78) −2.2 | 0.44 (0.52) 16 | 0.56 (0.54) 16 | 0.64 (0.52) 14 |
| your BTCUSDT 2024–26 | 44 | −2.8 % (−4.0 %) +0.9 | 1.30 (1.50) −0.3 | 0.64 (0.47) 14 | 0.56 (0.47) 9 | 0.33 (0.56) 3 |
| your ETHUSDT 2024–26 | 32 | −5.1 % (−4.8 %) −0.2 | 2.88 (1.50) +2.8 | 0.21 (0.48) 14 | 0.25 (0.53) 12 | 0.46 (0.51) 11 |

* **2024–26:** BTC's bottoms step down the way a random walk with the same volatility would (z within ±1).
  ETH's are *less* regular than random (dispersion z +2.8). There is no stable "x % per new low" to project.
  The training-only numbers in the notebook point the same way.
* **2012–18:** BTC's steps were larger and more regular than the null. But the null removes BTC's roughly
  3,000-fold rise and its long one-way runs (bubbles and crashes), so this mixes those with any level effect.
* **Bounces at the projected level are 50/50 within noise everywhere.** BTC is above 0.5 and ETH below, at the
  same steps, so neither direction can be a rule. The largest gap is ETH at 5 % (z −1.9, 12 legs), and that one
  is a failure to bounce. With ~40 statistics per market, one |z| near 2–3 is expected by chance.


