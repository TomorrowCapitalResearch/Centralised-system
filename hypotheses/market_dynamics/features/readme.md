# What the attached siglab run supports

This review uses the ten attached CSVs preserved under `research/prior_siglab`. They are earlier,
aggregate research outputs, not a new run of the strategy/regime extension.
`python tools/review_prior_results.py` reproduces the row counts and the summary JSON.

## Training picks did not survive this hold-out

| Training-selected strategy | Discovery Sharpe | Discovery CAGR | Hold-out Sharpe | Hold-out CAGR | Hold-out max DD | Hold-out trades |
|---|---:|---:|---:|---:|---:|---:|
| Thin drops, 15-minute, k=2, bottom 10%, maker, no extra stop | 1.28 | 0.27% | -1.01 | -1.22% | -3.75% | 232 |
| Sigma fade, hourly, w=50, k=3, opposite-band exit | 1.04 | 3.16% | -0.98 | -6.01% | -20.65% | 297 |

Source: `scoreboard.csv`. CAGR fractions have been converted to percentages; small positive discovery
returns are not large annual returns. The selected rows have 30 and 86 discovery trades respectively.

In `s1_books.csv`, all **48 hold-out variants have negative Sharpe** (best -0.11).
In `s3_books.csv`, all **48 hold-out variants have negative Sharpe** (best -0.47).
These tables do not establish that a profitable out-of-sample cell can be selected by another threshold tweak.
They motivate a new conditional hypothesis with a competing explanation, not activating the old unconditional fades.

## Price structure remains an unconfirmed conditioning variable

`s4_phases.csv` contains 120 rows across anchor windows, phases, horizons and splits.
Its adjusted q-values range from approximately 0.983 to 0.991: none provides conventional adjusted
significance for the tested phase-return effects. The hold-out z-statistics range from -2.01 to +1.43;
an isolated unadjusted statistic near two is not strong support after this search.

This does not establish that every higher-low or range-location strategy must fail. It means this run
does not justify declaring the phase itself predictive. The new lab exposes the features and tests both
boundary fades and breakouts, and publishes pivots only at their actual confirmation time.

The example bottom-10%, n=15, k=1 drop row recovered 79.7% of the time in hold-out,
versus 78.6% in its null. Its touched-rate z is 1.20 and its 6-hour excess-return z is -1.15
(`s1_events_ref_all.csv`). A high recovery probability alone is not a profitable-trade criterion.

## Coverage and accounting limits

The supplied event/phase tables show **two discovery coins and three hold-out coins**. The book tables
contain 365 discovery days and 1,030 hold-out days. This is narrower and less uniform than the older
20-coin edgelab summary. The aggregate exports do not identify the exact available calendar boundaries
or permit reconstructing a point-in-time asset universe. Keep symbol coverage fixed when comparing a new run.

The previous tables use siglab's book accounting. The current engine marks open inventory each execution
bar and reconciles signed P&L, fees and funding to cash. Aggregate trade summaries alone cannot audit
intrabar fills, equity marks or signal causality. No numerical result above certifies those mechanics.

There are no newly computed historical returns for the new conditional strategies in this delivery.
Generated demonstrations validate the software path; Mark's local candle archive is still required
to assess the actual matches.



# Data inputs and information clocks

Primary references checked 1 November 2026. Live acquisition is provided as runnable code but was not performed in this environment. Real replay uses only the supplied BTC minute archive. The thirty-asset run is synthetic.

| Family | Free route implemented | Useful current measurements | Actual clock / limitation |
|---|---|---|---|
| Crypto prices/volume | Binance public monthly ZIP + checksum; local per-asset archives | drift, volatility, shocks, ranges, participation | preserve 1 minute; no L2/L3 inferred from OHLCV |
| Universe | CoinGecko Demo/current ranking with explicit ID→pair mapping | declared membership and coverage | current snapshot is not a historical top-30 archive |
| Funding | `derivatives.fetch_funding` | settled funding/crowding context | settlement events; historical first-availability cannot be assumed from retrieval |
| Open interest | `fetch_open_interest` | quantity and notional changes, participation/leverage context | native minimum 5-minute history endpoint; provider recent-history limits apply |
| Futures basis | `fetch_basis_snapshot` | current mark/index deviation | receipt timestamp; per-symbol snapshot, not an invented basis backfill |
| Rates / yields / broad USD | `sources.fetch_fred` with free key and explicit vintage | level, changes, yield spreads | publication/vintage time; not reference-period midnight |
| FX | `sources.fetch_ecb_fx` | daily EURUSD / EURGBP / other reference context | ECB daily references, not executable minute FX quotes |
| Gold | optional yfinance GC=F or GLD | slower gold-related returns/risk context | futures continuation or ETF respectively, not spot; daily helper, not minute gold history |
| News/sentiment | no news collector implemented | optional future family | requires timestamped observations and incremental evidence |
| Order book | no L2/L3 collector implemented | actual spread/depth/imbalance later | record venue observations; volume/range remain proxies |

More information is useful only if available at decision time and incrementally informative. Start with thirty explicitly declared liquid non-pegged crypto assets. Add macro, derivatives and cross-asset groups separately. Preserve absent inputs rather than inventing observations to complete a feature matrix.

## External record format

`feature,value,observation_time,available_at,vintage(optional)`

`available_at` is the time the exact value could be consumed. Source event timestamps and client receipt timestamps differ. Derivatives collectors retain receipt provenance. Their default historical records become available at retrieval, not silently at an old funding/OI event time. The funding helper optionally permits an explicit event-latency assumption, labels it, and never calls that verified historical first availability.

FRED snapshots require an explicit vintage. Default availability is next UTC midnight after that vintage, since the date alone does not establish a precise intraday release. Fetch multiple vintages to reconstruct a release stream. ECB/gold current-history helpers default to retrieval-time availability, so they remain absent in earlier replay unless independent historical availability evidence is supplied.

## Source transformations

Example `feature_specs.json`:

```json
{
  "fred:DGS2": {"transform": "difference", "max_age": "5D"},
  "fred:DGS10": {"transform": "difference", "max_age": "5D"},
  "ecb:EURUSD": {"transform": "log_return", "max_age": "4D"},
  "gold_proxy:GC=F": {"transform": "log_return", "max_age": "4D"},
  "funding:BTCUSDT": {"transform": "level", "max_age": "12h"},
  "open_interest:BTCUSDT": {"transform": "log_return", "max_age": "20min"},
  "basis:BTCUSDT": {"transform": "level", "max_age": "5min"}
}
```

Review exact helper-returned feature names and units. A change of a yield expressed in percent is in percentage points; converting to basis points is an explicit transformation. A broad trade-weighted USD measure is not ICE DXY. Notional OI can change because prices change, so quantity and notional are separate features.

Transform `log_return` and `difference` over distinct source observation updates before carrying the result to minute decisions. A daily move must remain the most recently known daily move, not become zero at the next minute. Earlier-observation revisions do not replace a newer current observation. Known revised history can influence subsequent transformations only after the revision became available.

Maximum ages are feature-specific settings, not universal trading calendars. Weekend/holiday behavior must be reviewed. Expired data become missing/stale; an old observation should not receive a fresh timestamp simply because an endpoint is polled again.

## Primary references

- Binance archive fields, paths, microsecond change and checksums: https://github.com/binance/binance-public-data
- Binance derivatives market-data catalog (funding/OI/mark-index fields): https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/market-data
- CoinGecko current markets endpoint: https://docs.coingecko.com/demo/reference/coins-markets
- FRED observation/vintage API: https://fred.stlouisfed.org/docs/api/fred/series_observations.html
- ECB reference rates: https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html
- Gold proxy client: https://github.com/ranaroussi/yfinance
- Barndorff-Nielsen and Shephard, power/bipower variation: https://shephard.scholars.harvard.edu/publications/power-and-bipower-variation-stochastic-volatility-and-jumps

Minute resolution is the common decision clock, not a promise that every free source supplies historical minute observations.

