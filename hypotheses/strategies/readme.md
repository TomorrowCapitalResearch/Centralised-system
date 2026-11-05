Premises:
1. the trading strategy must survive trading fees (0.09% for taker, 0% for maker). This means we are more incline to do maker strategies.\
exchange is maker heavy.



Passive dislocation capture. This is LiqGapMR’s idea with a resting entry instead of a market buy. Keep post-only bids a set distance below a reference price (your Binance feed, or the index price the ticker returns) and re-centre them with replace. Fills happen only when Revolut X trades at a discount to the wider market, with no fee and no reaction race. The main risk is a market-wide drop filling every bid at once, so cap total exposure; this is also where a regime gate would pay for itself. 
revolut
Two-sided quoting on wide-spread pairs. Quote both sides and shift quotes down as inventory builds, with inventory running from zero to a cap. A taker paying 0.09% only profits from hitting a quote that is stale by more than that, which gives a slow maker some cover. It lives or dies on captured spread minus markout, per pair.
Slow long/flat allocation. Trend and regime positioning over hours to days across your top-30 universe, rebalanced with resting orders. Moves are measured in percent, so fees stop mattering; the hard parts are statistical power and that every position is the same BTC-beta bet.



| Family | description | Supplied evidence | Decision |
|---|---|---|---|
| BTC → altcoin minute lead–lag | x | x | x |
| Thin-volume drop reversion | x | x | x |
| Generic sigma-band fades | x | x | x |
| ETH wick ladder | x | x | x |
| Acceleration/participation continuation | x | x | x |
| Hourly trend | x | x | x |
| Structure-based routing | x | x | x |
| Old Bitstamp liquidity provision | x | x | x |
| Thin-volume drops revert | x | x | x |
| High-volume drops revert | x | x | x |
| Pushes into a level with rising or persistent volume | x | x | x |
| σ-band fades ("2 sigma") | buying 2–3 σ below the rolling norm and selling above it pays.|  x | x |
| ATH → global min → local max → local min | map the all-time high, the minimum since it (global min), the maximum since that (local max) and the minimum since that (local min, merging into the global min if broken); inside these periods behaviour repeats, and σ-band / stability entries work better once the slope has stopped falling. |  x | x |




| | rule (frozen from edgelab) | support |
|---|---|---|
| A1 wick ladder | one resting bid m × 1-minute σ below the last close (m = 8, 12, 16), target the pre-wick close, 1 h or 4 h limit, no bid in a minute after the leader fell ≥ 3σ | discovery pick net > 0 with t > 2 and z > 2 vs null; same sign in hold-out; positive in most of the grid |
| A2 heavy-participation bounce | ≥ 3 or 4 σ 5-minute drop, participation in the top third, maker bid at the event close, target the pre-move price, 4 h | 60-minute return and trade net above null (z > 2) in both periods |
| A3 lead–lag | leader (EUR/USD for FX, S&P 500 for indices, US 10Y for bonds) makes a ≥ 3σ minute the follower has not matched by 2σ of its beta-implied move; taker, 15 min | 15-minute follower return above null and net > 0 after costs |
| A4 trend | daily MA / time-series momentum (28, 56, 112 days), optional efficiency ratio > 0.2, inverse-vol weights, long-only and long/short | Sharpe above buy & hold in both periods, a broad block of lookbacks |
| A5 cross-sectional momentum | top 3 by 28/56-day return, raw and risk-adjusted, optional efficiency filter | beats equal-weight buy & hold in both periods |

Prior evidence: crypto only (edgelab RESULTS). The ladder and the heavy bounce are liquidity-provision trades; they
depend on how deep and how often a venue's book gets swept, so a different market structure is a real test.

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

# siglab results: other asset classes, the earlier alphas, risk overlays, corrected sigma (31 Oct 2026)

**Data.** 25 Oanda markets from the FutureSharks repository, 1-minute mid-price bars with tick-count volume,
Jan 2005 → 14 May 2020, 110 million bars: 6 FX pairs, 8 stock-index CFDs, 7 commodities, 4 government bonds.
Plus Bitstamp BTC/USD minute bars 2012 → Jan 2018 for the crypto alphas. Session handling: gaps over 30 minutes
are breaks; intraday trades go flat before them, hourly and slower trades hold through them.

**Method.** The grids in `siglab/studies.py` and `HYPOTHESES.md` (v2) were fixed before the runs. Volume profiles,
percentile thresholds and every pick come from **discovery 2005–2012**; **hold-out 2013 → May 2020** is read once.
Every statistic is compared with 10 sign-flipped copies of each market (same volatility, volume and gaps; direction
randomised): `z` = (real − null) / max(null sd, real day-clustered SE), so |z| < 2 is noise. Costs per leg are
placeholders (FX 0.75–1.25 bp, index CFDs 0.8–2 bp, commodities 1.5–10 bp, bonds 0.8–2 bp half-spread); a resting
order on mid quotes fills only after price trades half a spread through it. Books: 10 equal slots across all
markets. Reproduce: `python scripts/run_multi.py --part minute | coarse | btc` (CSV tables and summaries in
`results_multi/`).

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

Regime detection, stops and risk rules did not raise profits or Sharpe on top of these signals, with one exception:
stops helped on S1's intraday drops, where the moves persist, and only by losing less. Volatility targeting and
inverse-volatility sizing made the risk smoother, not the returns better. The one effect that survived both
periods (lead–lag) is real, but too small to pay for crossing the spread.

## Your crypto run (siglab 0.1, 30 Oct)

One thing first: the event tables count **2 coins in discovery and 3 in hold-out**, so siglab found far fewer coins
in your data folder than edgelab's 20. Treat the run as indicative; pointing `SIGLAB_EXTRA` at edgelab's Binance
folder gives the next run all 20.

| | discovery 2021–23 | hold-out 2024–26 | read |
|---|---|---|---|
| S1 touch rate vs null (k 1 / k 2) | 0.77 vs 0.79 / 0.64 vs 0.66 | 0.79 vs 0.79 / 0.66 vs 0.66 | the "recovery" is random-walk geometry |
| S1 6 h excess, bottom 20 % vs top 20 % participation (k 1) | −3.8 vs +8.6 bp | −4.5 vs +4.9 bp | the reverse of the hypothesis, as edgelab found |
| S1 trade cells net > 0 | 10 of 48 (the big ones on 4–86 fills) | 1 of 48 | |
| S1 pick (n 15, k 2, bottom 10 %, maker, no stop) | Sharpe 1.28 on 30 trades | −1.01 on 232 trades | small-sample pick |
| S1 with a 2σ stop, maker | win rate 73 % → 43 %, net +5.0 → −9.4 bp | 68 % → 46 %, −7.6 → −6.8 bp | book Sharpe −1.6 → −6.2 and −2.3 → −5.9 |
| S2 rising/persistent up-push breaks, 4 h | +31 bp (z up to 2.3) | +3 bp (z 0.3) | faded |
| S2 rising/persistent minus fading | 13 of 16 cells > 0, none t > 2 | 8 of 16 > 0 | no effect |
| S3 trade cells net > 0 | 6 of 48, none t > 2 | 0 of 48 | |
| S3 pick (1 h, w 50, k 3, exit at band) | Sharpe 1.04 | −0.98 | |
| S4 phase cells with \|z\| > 2 | 1 of 120 | | chance alone gives ~6 |

All four failed out of sample on crypto, both discovery picks reversed sign, and the stop made S1's book worse.

## S1: thin-volume drops revert (FX, indices, commodities, bonds)

20,000 to 580,000 drops per (n, k) cell and period (n = 5, 15, 60 minutes; k = 1, 2, 3 σ).

| | discovery 2005–12 | hold-out 2013–20 |
|---|---|---|
| touch rate of the pre-drop price within 6 h, all drops (n 15, k 2) | 0.65 (null 0.65) | 0.64 (null 0.65) |
| same, bottom 10 % participation | 0.65 (null 0.65) | 0.66 (null 0.66) |
| 6 h excess return, bottom 10 % / all / top 10 % (n 15, k 2) | −0.6 / +1.3 / +1.3 bp | +2.3 / +0.2 / 0.0 bp |
| event cells with 6 h excess z > 2 | 3 of 35 (two heavy, one thin) | 0 of 35 |
| trade cells net > 0 after costs | 3 of 48 (best +0.8 bp) | **0 of 48** (−2.0 to −5.4 bp) |
| best 10-slot book Sharpe | +0.11 | −0.40 |
| discovery pick (n 15, k 3, bottom 10 %, taker, 2σ stop) | Sharpe 0.11 | −0.69 |
| pick net per trade vs sign-flip null / random-entry twins | +0.8 vs −3.8 bp (z 1.7) / −4.0 bp | −2.9 vs −3.6 bp (z 0.5) / −3.5 bp |

* Thin drops don't come back more often than a random walk with the same volatility. Touch rates match the null to
  the percentage point in every bucket. 5-minute drops touch back slightly *more* often than the null (0.78 vs 0.77,
  z 3–4: one-minute quote noise); 15–60-minute drops slightly *less* often (z −2.5 to −5.4: short-term continuation).
* Excess returns after drops are mostly 0–3 bp at 6 h. The largest, after thin commodity drops in the hold-out
  (+6 bp, t 2.7; +3 bp, t 0.7 in discovery), is smaller than a commodity round trip (6–20 bp). Index 3σ drops kept
  falling in the hold-out (−2.8 bp at 6 h, t −2.3).
* **Stops helped here, which is what the theory predicts for these markets.** A 1–2σ stop beat no stop in 28 of 32
  matched cells in discovery and 31 of 32 in the hold-out (net −3.5 vs −4.4 bp per trade, win rate 30 % vs 58 %),
  because 15–60-minute drops on these markets tend to keep going for a while (touch rates below the null). A stop
  adds value when moves persist, which is exactly when a reversion trade is wrong. It made a losing trade lose less;
  nothing turned positive.

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

## What to run next on your PC

1. **Fresh data:** `python scripts/fetch_dukascopy.py --data "C:/Users/Mark Brezina/data" --start 2015-01 --all`
   (2015 onward, with bid/ask extremes; a long download), then
   `python scripts/run_multi.py --data "<data>/dukascopy" --symbols all --splits discovery:2015-01-01:2020-06-01 fresh:2020-06-01:`.
   Picks are re-made on Dukascopy 2015–20 and read once on June 2020 onward. Nothing above has seen 2020–26.
2. **All 20 coins:** set `SIGLAB_EXTRA` to edgelab's Binance folder and rerun `run_multi.py --universe crypto`.
   Your 30 Oct run found only 2–3 coins.
3. **Revolut X fills:** the Bitstamp ladder shows the profit depends on the venue's book. Log real resting-order
   fills before trusting any limit-order backtest.
4. **The structure ideas** moved to the Strategy Regime Lab (notebook 05, cascading extrema). See its
   EXTREMA_RESULTS.md.

Sources for the overlay theory: Kaminski & Lo (2014), *When do stop-loss rules stop losses?*, Journal of
Financial Markets 18. Moreira & Muir (2017), *Volatility-Managed Portfolios*, Journal of Finance 72(4).
Harvey, Hoyle, Korgaonkar, Rattray, Sargaison & Van Hemert (2018), *The Impact of Volatility Targeting*,
Journal of Portfolio Management 45(1).

# Cascading extrema: first results (31 Oct 2026)

What was tested: the ATH → global min → global max → local min → local max map from your charts, made causal
(`backtest/extrema.py`). It was used three ways: as a regime detector (up / flat / down segments between structure
breaks, falling legs, near ATH), as a bounce-level study (do new lows step down by a regular %, and does price
bounce at the projected next level?), and as strategies (`extrema_bounce`, `segment_band`), alone and routed by
structure state. Reproduce everything with `notebooks/05_cascading_extrema.ipynb`. The executed copy on your
BTC/ETH data is `notebooks/executed/05_cascading_extrema_your_btc_eth.ipynb`.

## Your round 01 (v1.0.2, notebook 03, BTC / ETH / SOL, 15-minute strategies)

| | train | validation | test |
|---|---:|---:|---:|
| routed policy (every state OFF) | 0 trades | 0 trades | 0 trades |
| unfiltered `trend_follow` 16/64, 15 min: Sharpe | −0.65 | −2.66 | −0.55 |
| trades / net per trade | 1,425 / −7.4 bp | 477 / −27.1 bp | 476 / −5.6 bp |
| total return / max drawdown | −10.7 % / −18.8 % | −12.3 % / −14.3 % | −2.9 % / −10.5 % |
| mean daily return, 95 % block-bootstrap interval | −0.018 % (−0.061, +0.020) | −0.067 % (−0.122, −0.013) | −0.014 % (−0.076, +0.060) |

No strategy/state cell met the training bar (≥ 30 accepted trades, positive net, positive Sharpe), so the
policy turned everything off. That was the correct outcome. The unfiltered baseline lost in all three periods, and
in validation the interval excludes zero. Taker fees alone came to $2,458 in training, more than twice the
$1,068 loss. The 15-minute variants trade too often for 9 bp taker + 2 bp spread. The same trend rule on
hourly bars trades about a sixth as often (below).

## Data used here

* **Your BTCUSDT and ETHUSDT, 2024-01 → 2026-09.** The close path was rebuilt exactly from the hourly log
  returns in the two round-01 `*_regimes.csv` files. The starting price is approximate and only rescales levels.
  There are no wicks or volume, so limit fills are conservative and the maker volume cap is off. Frozen dates:
  train 2024-01-08 → 2025-08-10, validation → 2026-02-19, test → 2026-09-01.
  SOLUSDT was not included (no regimes CSV attached).
* **Bitstamp BTC/USD, 2012-01 → 2018-01**, hourly from minute bars. edgelab never used these data.
  Train 2012-01-31 → 2015-08-24, validation → 2016-10-30, test → 2018-01-08.
* **Account:** $10,000, 10 slots, costs 0 bp maker / 9 bp taker / 2 bp spread, limits must trade 5 bp through.
  Structure: pivot_k 2, min_pct 2 %, slope window 7 days, slope cut 1, min segment 12 hours. These were fixed
  before any run.

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

## 2. The structure states describe the chart; they barely predict it

* **Time shares:** flat 51–57 %, up 12–17 %, down ~8 %, near ATH 7–16 %, falling legs 2–11 %, transitions 1–4 %.
* **Spell length** on your data: down ~9–13 h, up ~11–16 h, flat ~26–34 h, falling legs ~25–60 h.
* **Next-24-hour return by state** (notebook §2) changes sign between periods: BTC falling legs +102 bp
  (train), −66 bp (validation), +73 bp (test). Only BTC down-segments were negative in all three periods
  (−38, −16, −54 bp). Those come from overlapping 24-hour windows and ~10-hour spells, so this is a lead, not a
  signal.

## 3. The strategies on their own (Sharpe train / validation / test, trades)

| strategy | your BTC+ETH 2024–26 | trades | Bitstamp BTC 2012–18 | trades |
|---|---|---|---|---|
| `extrema_bounce` 3 % below the broken low | −0.25 / −1.58 / −0.42 | 20 / 10 / 2 | −1.31 / – / −1.43 | 17 / 0 / 8 |
| `extrema_bounce` 5 % | −0.72 / −2.80 / +0.04 | 15 / 8 / 2 | −0.58 / – / +0.29 | 16 / 0 / 11 |
| `extrema_bounce` 8 % | −1.99 / −1.99 / −1.37 | 11 / 6 / 1 | −0.41 / – / +0.03 | 15 / 0 / 8 |
| `extrema_bounce` 12 % | +0.10 / −1.58 / +1.02 | 10 / 5 / 1 | +0.74 / – / +1.48 | 15 / 0 / 5 |
| `segment_band` k 2, all slopes | −1.43 / −2.48 / −2.44 | 138 / 47 / 46 | −1.13 / −1.48 / −1.55 | 232 / 53 / 35 |
| `segment_band` k 2, flat segments only | −1.50 / −1.68 / −0.92 | 112 / 37 / 37 | −0.98 / −1.45 / −1.37 | 203 / 41 / 32 |
| `trend_follow` 16/64, hourly | **+1.01 / −0.40 / +0.17** | 238 / 70 / 79 | **+1.37 / +0.96 / +3.05** | 303 / 107 / 105 |
| `mean_reversion` w 32 k 2, hourly | −0.02 / −2.96 / −1.58 | 451 / 153 / 141 | −1.36 / −2.49 / −1.32 | 530 / 133 / 164 |

* **The standard-deviation play inside stable segments lost in all 12 cells** after costs, including flat
  segments only.
* **`extrema_bounce`** trades rarely (0–20 times per period) and mostly loses. The 12 % step was positive in
  training and test on Bitstamp, on 15 and 5 trades. Bitstamp's validation period had no new lows at all.
  That is far too few trades to act on.
* **Hourly trend following** is the only rule with a consistent sign on old BTC. On your 2024–26 data it made
  money in training and roughly nothing afterwards.

## 4. Routing by structure state does not beat the plain trend

Training picks one variant per state (or OFF). Validation and test replay that frozen map in one account.

| Sharpe | train | validation | test |
|---|---:|---:|---:|
| your BTC+ETH: routed / trend alone | 1.95 / 1.01 | **−1.44 / −0.40** | 0.30 / 0.17 |
| Bitstamp: routed / trend alone | 1.46 / 1.37 | **−0.22 / +0.96** | 3.38 / 3.05 |

Frozen maps — your data: down → `segment_band` k 1.5; falling → `mean_reversion` k 2; flat, near ATH and
transition → `trend_follow` 16; up → `mean_reversion` k 2. Bitstamp: down → `segment_band` k 1.5; flat and
near ATH → `trend_follow` 16; up → `trend_follow` 8; falling and transition → OFF. The training gain is what
any selection produces. In validation, routing lost to the plain trend in both markets.

## 5. Your map, declared before it was run (nothing fitted)

| Sharpe train / validation / test | your BTC+ETH | Bitstamp |
|---|---|---|
| trend only in up / near-ATH structure | 0.81 / −0.95 / −1.78 | 1.34 / −0.15 / 2.66 |
| structure map: trend in up / near ATH, band in flat, bounce bid in falling legs, nothing in down | −0.22 / −2.90 / −1.85 | 0.73 / −0.84 / 1.78 |
| trend alone, for comparison | 1.01 / −0.40 / 0.17 | 1.37 / 0.96 / 3.05 |

Both maps are worse than the plain trend in every period, on both datasets. Keeping trend entries only for
up-structure drops the entries that start new trends (out of flat segments and falling legs) and keeps the
late ones.

## What this means

1. The cascade labels the chart the way you described it, using only past data, and it is ready as a regime
   axis or a risk context. On these data it carries no information that improves the trades.
2. "Each new bottom a solid % lower" did not survive the random-walk comparison on 2024–26. The bounce at the
   projected level is a coin flip within noise. Swing highs (your "similar behaviour for the local highs") do
   not step down systematically either. The median step between successive confirmed swing highs in training
   is +0.6 % for BTC (62 swings) and −0.4 % for ETH (68).
3. Mean reversion to the segment's own mean ± k·sd loses after costs, in all slopes and in flat segments.
4. The only consistent result is hourly trend following on BTC 2012–18. On your recent data it is weak, so it is
   not yet a dependable edge either.

## Next round: declare these before running

* Run notebook 05 on your archive with `REPLAY_TIMEFRAME = "1h"`, so real highs and lows decide the
  `extrema_bounce` fills, and include SOLUSDT. Change nothing else.
* **Support for the level idea:** bounce above the null with z > 2 in training for both BTC and ETH, the same
  sign in validation, and ≥ 20 legs pooled.
* **Support for routing:** validation Sharpe of the routed account above the train-selected unfiltered
  baseline, and test not worse. No change after reading validation.
* **Truly fresh data** is only what comes after 2026-09-01 (paper trading).

## Caveats

* 2024–26 has been inspected in earlier rounds (edgelab, round 01). It is a research period, not a hold-out.
* "ATH" means the highest close in the loaded history, which starts in January 2024. For ETH that is not the
  2021 high, so its cycles before the real new high of August 2025 are cycles of a 2024 high. Loading earlier
  history (your Binance archive back to 2017–2020) changes those cycles. Do that before reading the level
  statistics as ETH's own.
* Close-only reconstruction: no wicks, no volume. Real hourly bars will fill more limit orders.
* Bitstamp 2012–15 spreads were wider than the 2 bp assumed, and its volume is a placeholder.
* `extrema_bounce` and the bounce statistics rest on 1–20 events per cell.
* Many statistics were computed. Read the pattern across markets and periods, not single cells.

