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
