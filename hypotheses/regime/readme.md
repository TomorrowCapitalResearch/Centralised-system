
# Conditional strategy research — rounds 1.1 and 1.2

The objective is a frozen state → strategy/OFF policy that improves a shared account after costs.
The proposed matches below are hypotheses. Flat price action need not mean-revert, and an extreme
price deviation does not imply an equal opposite move is coming.

Open `notebooks/04_structure_and_regime_strategies.ipynb`, run from the top, and begin with QUICK.
The package retains the UTC timestamp, monthly-Parquet and stale-kernel repairs from 1.0.2.
Main notebooks default to Mark's local archive; the separate executed demo notebooks use generated data.

## Separate the observations

| Observation | Definition available at the completed bar |
|---|---|
| Trend | `uptrend`, `downtrend`, `range`, `transition`; trailing movement / volatility and path efficiency |
| Volatility | `low`, `normal`, `high`; thresholds fitted on the training quarter/three-quarter volatility quantiles |
| Price location | `near_low` / `middle` / `near_high`; bottom/top 20% of the declared rolling close-price range |
| Observed ATH / ATL | Expanding maximum/minimum close of supplied history; not a claim to full listing history |
| Global high / low | Maximum/minimum close of the explicit lookback, e.g. 7, 30 or 90 days |
| Low since anchor | Lowest close since the active anchor high in the online phase map |
| Local high / low | Zigzag extreme published only when the reversal threshold has been met |
| Phase | Near high → falling → rebound → pullback → higher-low bounce; an online descriptive state |
| Flat upper/lower boundary | Small trailing variation in candle highs/lows relative to per-bar volatility × sqrt(window) |

The structure detector returns all these fields. Policy axes default to **trend × volatility**,
so that location/phase can be inspected without fragmenting the training sample into hundreds of cells.
`state_axes=["trend","volatility","location"]` or including `phase` is an explicit new experiment.
Labels computed on an hour ending 11:00 become usable at 11:00; they are not visible inside that hour.
Local pivot confirmation is delayed deliberately; the previous turning-point timestamp is never backfilled.

## Proposed matches to test

| Proposed state/context | Strategy | Entry | Exit / competing explanation |
|---|---|---|---|
| Range, low/normal volatility | `band_reentry` | Re-enter the lower σ band after a dip; mirror above upper band for research shorts | Fixed signal-time mean, stop beyond band, time limit |
| Range, high volatility | `band_limits` | Rest a bid at lower band; research ask at upper band | Mean or opposite band; stop beyond entry band; adverse-selection and fill stress |
| Stable support/resistance | `flat_boundary_limits` | Passive floor bid / ceiling ask | Opposite boundary, buffered stop, time limit |
| Same flat boundaries | `flat_boundary_breakout` | Completed close through previous ceiling/floor | Opposite boundary stop, time limit; tests breaks instead of assuming bounces |
| Near range low, confirmed higher low | `structure_reversal`, long | Confirmed low above preceding confirmed low, in bottom 40% of range | Known opposite pivot or midpoint, buffered low stop |
| Near range high, confirmed lower high | `structure_reversal`, short | Mirror at upper 40% | Known low/midpoint, buffered high stop |
| Uptrend | Existing `trend_follow` | Cross fast average while fast > slow | Fast < slow, stop or time limit |
| Downtrend | `downtrend_pullback_short` | Falling MA ordering, a recent rally, then a down close | Previous rolling low, stop or time limit |
| Downtrend | `downtrend_breakdown_short` | Close crosses previous rolling low | Stop or time limit |
| Weak / unsupported state | OFF | No new entry | Current inventory keeps its declared exits unless regime-change exits were selected |

The learned policy evaluates each candidate in every observed training state. A proposed match is
not enforced or endorsed. The manual `Lab.run_policy` interface permits a stricter predeclared map,
including downtrend → None for spot. Spot starts in cash: it sells only inventory it bought earlier.
An existing asset holding or a hold-and-rebuy benchmark would need an explicit initial-inventory policy;
the lab does not credit an unfunded sale. The linear proxy can sell short and buy back later.

## Clock-time horizons and search size

QUICK uses 15-minute signals, k=2, w=32 and a 16-bar hold (8-hour norm / 4-hour maximum hold).
Turning QUICK off declares 15-minute / 1-hour / 4-hour signal horizons, k=1.5/2/2.5,
and 4/16-bar holds for the band families. Other hypotheses retain one parameter choice per horizon.
These alternatives belong to one training selection. Do not choose the best horizon from final test results.
The notebook prints the full declared grid before running it.

Bands use log-price mean μ and sample standard deviation s:
lower=exp(μ−k·s), upper=exp(μ+k·s). This s is the dispersion of prices in the window,
not the same measure as the per-bar return σ used by the wick ladder or pivot detector.
Targets and stops in the new band strategies are fixed when submitted, rather than moved using future data.
Passive entries are refreshed at each signal bar, with one-bar TTL. `side="both"` chooses one offered side
using the current price relative to the mean; simultaneous two-sided quotes and hedged positions are not modeled.

Initial execution uses 15-minute bars for runtime. Recheck the frozen shortlist on minute execution,
then venue data, using the original training models and policy. The candle execution path assumes a stop
first if barriers conflict, permits no entry-bar take-profit credit, and requires strict limit trade-through.
Neither resolution establishes queue position, partial exit fills, latency or capacity.

## Freeze selection and test the actual account

1. Declare symbols, dates, source history, account mode, regime axes, strategy grids and costs.
2. Fit regime thresholds on training. Replay each candidate/state as its own funded training account.
3. Require enough accepted trades, positive average net basis points and profit; choose the training score
   winner per state or OFF. Training profits are selection-biased and are not confirmation.
4. Replay the combined frozen map on validation and later test; all accepted trades share the same slots/cash.
5. Compare with the train-selected unfiltered candidate, OFF/cash and a separately specified buy-and-hold
   benchmark if required. Inspect net return, drawdown, exposure, costs, direction and sample counts.
6. Stress the same frozen choices: wider spread, deeper trade-through, positive and negative funding,
   and alternative execution resolution. Preserve the original design if the stressed results fail.
7. Require a later untouched or forward-paper period after research changes. The previously inspected
   2024–26 dates cannot become fresh just by changing a notebook name.

`Lab.replay_frozen` reuses the existing fitted models and exact selected variants, without re-fitting:

```python
from dataclasses import replace
stress = account.replay_frozen(experiment,
    start=VALIDATION_END, end=TEST_END,
    costs=replace(account.costs, spread_bp=5, funding_bp_per_8h=-1))
display(stress["regime_policy"].summary())  # linear_proxy funding stress only
```

To test minute execution, construct another Lab from the same underlying minute history and original symbols,
then call its `replay_frozen(experiment, ...)`. Keep warmup/history identical; truncating the beginning changes
observed-history extrema. This method accepts an in-memory Experiment, not an arbitrary saved JSON plan.

## Research short accounting

`Execution(market="linear_proxy")` is a fully collateralised **1x linear-contract proxy**, not borrowed spot.
Entry reserves quantity × entry price plus fees. Open collateral is
quantity × [entry price + side × (mark − entry price)] − accrued funding.
Positive funding rates charge longs and credit shorts; negative rates reverse the transfer.
Realized net P&L equals signed reference-price P&L minus spread, fees and funding, and reconciles to cash.
Reports export direction attribution and funding separately.

Use `Lab(..., funding={symbol: Series}, mark_bars={symbol: OHLC DataFrame})` for recorded UTC funding events
and separate mark candles. Decimal funding rate 0.0001 is 1 bp. Supply every symbol; empty funding Series
explicitly means no events for that symbol. Do not supply a constant funding stress rate as well.
Funding applies to positions held before an event, before known-at-open exits; entries at that timestamp
are excluded. Events between execution-bar opens are rejected instead of silently rounded.

The notebook's constant 1 bp / 8h and 0.5% maintenance ratio are stress assumptions, not a current exchange
schedule. Spot OHLC substitutes for marks unless separate history is supplied. Liquidation uses that simple
maintenance proxy; crossing liquidation and a stop in one bar conservatively gives liquidation priority.
An insolvent account raises an error rather than clipping the loss. The proxy omits tiered maintenance,
leverage, borrow availability, basis forecasting, ADL and exchange-specific settlement. Funding/mark histories
do not turn it into an audited venue simulator.

Primary documentation checked 31 Oct 2026:

- [Binance funding history, funding event time and associated mark price](https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/market-data)
- [Binance liquidation: mark versus last price and maintenance margin](https://www.binance.com/en/support/faq/detail/360033525271)

The 1.1 strategy/regime tests were exercised on generated data only; no historical backtest of those
matches has been run against Mark's local candles. See `INPUT_RESULTS_REVIEW.md` for the limited conclusions
supported by the earlier siglab run.

# Round 1.2 — cascading extrema

Open `notebooks/05_cascading_extrema.ipynb`. Definitions are in the README section "Cascading extrema". The
structure parameters (pivot_k 2, min_pct 2 %, slope window 7 days, slope cut 1, min segment 12 hours) were fixed
before the first run. Changing them is a new round, not a tweak after reading validation.

## Hypotheses and what counts as support

| | Claim (Mark) | Statistic | Support |
|---|---|---|---|
| E1 | Each new global min between ATHs is a fairly constant % below the previous one | median `low_step` and its dispersion (IQR ÷ median), vs 30 sign-flipped copies | dispersion below the null (z < −2) in training for BTC and ETH, same sign in validation |
| E2 | Price bounces at the projected next level (broken low × (1 − step)) | share of legs that rebound theta before falling another theta from the first touch | above 0.5 and above the null (z > 2) in training, same sign in validation, ≥ 20 legs pooled |
| E3 | Local highs step down by a similar % | median step between confirmed swing highs, and its dispersion | as E1 |
| E4 | Between breaks, stable segments are up / flat / down, and strategies fit them (trend in up, σ-band in flat) | trained routing; and the declared map, which is not fitted | validation Sharpe above the train-selected unfiltered baseline, test not worse; the declared map beats the plain trend in at least two of three periods |
| E5 | The σ-band "standard deviation play" works inside segments | `segment_band` alone, all slopes and flat only | positive net bp after 0 / 9 + 2 bp costs in validation and test, ≥ 30 trades each |

A random walk with the same volatility produces bottoms that step down, ~50 % bounces and plenty of "flat
segments". The sign-flipped copies are the yardstick for E1–E3. For E4–E5 the yardstick is the plain
trend/unfiltered account replayed under the same costs, not cash.

## First results (details in EXTREMA_RESULTS.md)

E1, E2 and E5 are not supported on BTC/ETH 2024–26. For E3, swing highs show no systematic step-down (median
+0.6 % BTC, −0.4 % ETH, 60+ swings each); their dispersion has not yet been compared with the null. E4's
trained routing lost to the plain trend in
validation on BTC/ETH 2024–26 and on Bitstamp BTC 2012–18, and the declared map was worse than the plain trend
in every period on both. 2024–26 is a research period, not a fresh hold-out. The rebuilt close-only bars
understate limit fills, so rerun on hourly bars with real highs and lows from the archive before anything
is retired.
