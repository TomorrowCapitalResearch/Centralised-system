# 02 — Market States, Microstructure, Signal Processing & Alpha Hypotheses

> **Goal:** turn the images' quantitative insights and the user's ideas into separately testable modules: state inference, volatility, order-book behavior, spread/structure analysis and strategies. Nothing here is a verified profitable strategy.

## 1. Research universe and data specification

### Intended universe

- **Crypto spot**: core BTC/ETH plus liquid alts; start with 5–11 verified symbols, expand toward 20–30+ once liquidity and data correctness are established. One-minute OHLCV data are a useful research base; they cannot reconstruct full intrabar queue states.
- **Optional futures/perpetuals**: mark price, index, futures price, basis, funding, open interest, liquidation events and matched timestamps.
- **Cross-venue price discovery**: Binance spot/futures versus execution venue (e.g. Revolut X); check quote conventions, genuine latency, venue access and trading feasibility.
- **Market microstructure**: best bid/ask, depth ladder, trades with aggressor flags (if available), additions/cancellations, book messages, trade IDs, sequence counters and snapshots.
- **Macro/exogenous**: FX, gold, rate curves, treasury yields, equity indices, credit proxies, US opening sessions, news/surprise information with point-in-time release timestamps.
- **Physical-market analogy**: oil storage capacity utilization, inventory, futures term structure, convenience yield, contango/backwardation. Do not treat these as identical to exchange crypto order-book depth.

### Data contract

```yaml
asset: BTCUSDT
venue: example_venue
frequency: 1m
timezone: UTC
bar_timestamp_rule: end_exclusive
fields: [open, high, low, close, volume]
additional_fields: [quote_volume, n_trades, taker_volume]
record_quality: [is_missing, is_stale, is_duplicate, is_outlier]
corporate_actions_or_contract_changes: null
availability_time: explicit
raw_source_checksum: required
```

Research must distinguish transaction time, event time, data receipt time and first time usable by a real strategy. Backtesting on data published only later introduces look-ahead even if file rows are chronological.

## 2. State model: multiple axes, not one label

An actionable market description can be a **joint or factorized posterior** of distinct axes rather than one label:

| Axis | States / continuous metrics | Observables |
|---|---|---|
| Direction | up, down, flat, undecided | slopes, trend t-stat, directional persistence, returns |
| Volatility | low, medium, high; forecast distribution | EWMA, GARCH, realized/range variance, volatility of volatility |
| Mean reversion | fast/slow/absent; half-life | residual autocorrelation, AR coefficients, Ornstein–Uhlenbeck fit |
| Turbulence | calm, discontinuous, jump clustering | standardized jumps, cross-asset covariance breaks, tail risk |
| Liquidity | deep/shallow, balanced/imbalanced | spreads, depth, queue depletion, market impact, replenishment |
| Breadth | broad/narrow participation | fraction of assets rising, above MAs, volume diffusion |
| Cross-asset network | concentrated, fragmented, synchronized | graph centralization, clustering, spectral gaps, correlation shifts |
| Macro | growth/inflation/liquidity/policy conditions | publication-timestamped data and derivatives responses |
| Market structure phase | candidate phase 0–4 | ATH, global/local extrema, range, breakout/retracement logic |

### Proposed output schema

```json
{
  "as_of_utc": "YYYY-MM-DDTHH:MM:SSZ",
  "symbol": "BTCUSDT",
  "direction": {"label": "undecided", "probs": {"up": 0.35, "down": 0.31, "flat": 0.34}},
  "volatility": {"label": "medium", "probs": {"low": 0.20, "medium": 0.55, "high": 0.25}},
  "liquidity": {"label": "unknown", "confidence": null},
  "mean_reversion": {"strength": null, "half_life_bars": null},
  "jump_intensity": null,
  "breadth": null,
  "likely_next_state": null,
  "forecast_horizon_bars": 10,
  "model_version": "example",
  "diagnostics": {"data_coverage": null, "calibration_version": null}
}
```

The numerical probabilities above are **schema placeholders, not predictions**. A retrospective state label must never be reported as a calibrated forecast. Multi-axis probabilities need not multiply independently; estimate dependencies or report marginals honestly.

### Suggested inference ladder

1. Transparent deterministic baseline: rolling trend/volatility and market breadth.
2. Filtered probabilistic model: HMM/regime-switching models with likelihood and persistence.
3. Dynamic state-space or Bayesian filtering: separate latent market state, observation noise and transition uncertainty.
4. Sequence model: boosted trees, TCN/RNN, transformer, contrastive or rough-path representation if incremental skill is demonstrated.
5. Explicit calibration: state probabilities forecast *future* labeled outcomes, assessed with log loss, Brier score, reliability diagrams and transition timing.

## 3. Feature library (non-exhaustive but implementation-ready)

### Price / distribution

- Log returns `r_t=log(C_t/C_(t-1))`, cumulative log return and rolling aggregation.
- Close/open range, high/low range, body/wicks, candle fill, close location within range.
- Skewness, kurtosis, empirical tails, VaR/CVaR, tail exceedances and drawdown histories.
- Rolling linear slope, t-statistic, acceleration/second difference, cumulative slope from anchor, EMA/SMA/KAMA, moving-average spread.
- All-time high, latest confirmed global min/max and local extrema **detected causally**, not using future-centered peak detection.
- Mean-return features at 1–3 specified horizons; simple positive/negative bar counters.

### Trend and reversion

- AR(1) return autocorrelation, volatility-adjusted momentum, trend slope, directional consistency.
- Rolling/expanding Hurst exponent with known finite-sample issues.
- OU half-life for a **stationary residual or spread**, not arbitrary trending prices.
- ADF, KPSS, ARCH LM and change-point probabilities.
- Detrended log price using a **past-only** fit. A regression of all observations from 0 to final `T` cannot be used to generate a past trading signal without leakage.
- Bollinger bands/standard deviation distances, residual z-scores, 0-tangent/flattening of slope and action-like features (research hypotheses, not established finance laws).

### Volume and microstructure

- Volume mean-reversion, surprise = observed/forecast volume, volume imbalance, trade count, acceleration.
- Signed order flow, order-flow imbalance, depth imbalance, replenishment, cancellations, spread, microprice, realized spread, impact and resiliency.
- Cross-venue differences and futures lead/lag after synchronization.
- Time-of-day: session opens, US opening 1–30 minute windows and liquidity cycles (validate conditional probabilities, not just average returns).
- Jump amplitude and jump clustering using robust jump definitions; avoid creating jumps merely by rounding timestamps.

### Volatility and distributional uncertainty

- Close-to-close standard deviation, EWMA variance, range-based Parkinson, Rogers–Satchell, Yang–Zhang.
- ARCH(q), GARCH(1,1), EGARCH, GJR-GARCH; Student-t innovations if appropriate.
- Realized variance from high-frequency returns, volatility-of-volatility, vol term structure, forecast dispersion.
- Spectral entropy, return entropy, MI and conditional MI, transfer entropy, Wasserstein distance between rolling distributions, factor uncertainty.

### Cross-asset/network

- Rolling pairwise or partial correlation; rolling eigenvectors/eigenvalues; covariance shrinkage and common factor loading stability.
- Centrality: eigenvector, degree, closeness, betweenness, PageRank, Katz; k-core and eccentricity.
- Global: clustering, modularity, assortativity, centralization, path length, density, connected components, network similarity over time.
- Signed correlations cannot be naively treated as positive distances or spring stiffness; choose appropriate weight transformations and verify positive semidefiniteness where needed.

## 4. Volatility models and Q-variance

Conditional return model: `r_t=μ_t+σ_t z_t`, innovations with specified distribution.

| Model | Equation / mechanism | What it tests |
|---|---|---|
| ARCH(q) | `σ²_t=ω+Σ_i α_i ε²_(t-i)` | Lagged squared shocks |
| GARCH(1,1) | `σ²_t=ω+αε²_(t-1)+βσ²_(t-1)` | Persistence / clustering |
| EGARCH | Model `log σ²_t` plus signed standardized shock | Asymmetric conditional volatility |
| GJR-GARCH | Add `γ ε²_(t-1) 1{ε_(t-1)<0}` | Negative-shock asymmetry |
| EWMA | `σ²_t=λσ²_(t-1)+(1-λ)r²_(t-1)` | Simple adaptive baseline |
| Stochastic volatility | Latent stochastic variance process | Inference plus variance-of-variance |

**Factor uncertainty index** screenshot: EWMA `λ=0.85`, GARCH(1,1), RV12 and stochastic-volatility estimators show historical time series with common spikes around major turbulence. The chart alone does not establish predictive factor-return power. A proposed factor uncertainty metric should be compared with VIX-like proxies, realized vol and factor covariance instability under predictive cross-validation.

### Q-variance / David Orrell challenge [PAPER / HYPOTHESIS]

Canonical challenge: [github.com/q-variance/challenge](https://github.com/q-variance/challenge). The precise published **empirical fitting relation** described there is

$$
\sigma^2(z)=\sigma_0^2+\frac{(z-z_0)^2}{2},\quad z=\frac{x}{\sqrt T},
$$

where `x` is drift-adjusted log price change for interval `T`, and `z_0` allows asymmetry. Distinguish observed/expected variance **conditioned on period return** from instantaneous conditional volatility in a standard GARCH forecasting model. The competition asks for a continuous-time process reproducing the relation with at most three free parameters; reproducing the plot on one dataset is not enough.

**Research plan:** estimate dependence across coins, stocks/FX if licensed, weekly versus minute-to-daily scales; bootstrap bands; check sampling/selection bias; compare diffusion, jump, stochastic-vol, regime mixtures and known bridge-conditioning effects; use fresh out-of-sample data. A nice parabola is not itself a trading edge.

## 5. Multi-horizon crossing / structural state

A photographed whiteboard appears to use signals at approximate horizons **6, 20 and 60**:

$$
\text{Buy if }g_6<g_{20}\ \&\ g_6<g_{60},\quad
\text{Sell if }g_6>g_{20}\ \&\ g_6>g_{60}.
$$

Exact variable/units are not recoverable from the image. Test on different defined inputs: MA value, detrended residual, slope, momentum and volatility; do not silently label this trend-following. Add exit rules, stops and a flat zone. Include reversed signals and random-entry twins.

### Structural market phases (candidate)

- Phase 0: range formation or undecided after break.
- Phase 1: increasing highs/confirming trend.
- Phase 2: pullback or consolidation.
- Phase 3: lower low/failed breakout or trend exhaustion.
- Phase 4: recovery/new confirmed pivot.

This numbering is a proposed working taxonomy, not an established scientific standard. **Extrema only become actionable when confirmed by available past bars**. Separate discovery description from causal classification.

## 6. Market microstructure: queue diffusion and first-passage probability

Screenshot formulation begins with high arrival rates approximated via a functional CLT:

$$
 N_t\approx \lambda t+\sqrt{\lambda}W_t
$$

(up to scaling of time/point-process intensity) and queue dynamics

$$
\begin{aligned}
 dX_t&=m_Xdt+\sigma_X dW_t^X,\\
 dY_t&=m_Ydt+\sigma_Y dW_t^Y,\\
 dW_t^XdW_t^Y&=\rho\,dt.
\end{aligned}
$$

`X` = best bid queue and `Y` = best ask queue. Screenshot sketches drifts `m_X=h(λ_b-μ_b)`, `m_Y=h(λ_a-μ_a)`, conditional variances including liquidity shift intensity `η`, and (in a symmetric toy model) negative `ρ=-2η/(λ_b+μ_b+2η)`.

Define `u(x,y)=P(ask queue hits 0 before bid queue | x,y)`. For zero drift and equal unit volatility:

$$
 u_{xx}+2\rho u_{xy}+u_{yy}=0,
 \quad u(x,0)=1,\ u(0,y)=0.
$$

In the limiting degenerate case `ρ→-1`, with matched queue diffusion amplitudes and relevant absorption assumptions:

$$
 u(x,y)=\frac{x}{x+y}.
$$

This maps displayed bid depth dominance to an **upward next-price-change hitting probability in an idealized model**. It is not a calibrated probability in a real order book with variable quote size, correlated events, cancellations, hidden queues or multiple tick levels. The screenshot's full arctangent expression should be independently derived and boundary-tested before implementation; do not copy an unverified cropped formula into production.

### Hidden liquidity, asymmetry and non-identifiability

True queues: `B=x+H_b(x,y)`, `A=y+H_a(x,y)`; stylized probability

$$
 p(x,y)=\frac{x+H_b(x,y)}{x+y+H_b(x,y)+H_a(x,y)}.
$$

**Correct rearrangement**:

$$
(1-p)H_b-pH_a=p(x+y)-x
$$

so that, for `p>0`,

$$
H_a(x,y)=\frac{x-p(x+y)+(1-p)H_b(x,y)}{p}.
$$

Thus one observed hitting probability leaves a continuum of possible `(H_b,H_a)` pairs (subject to feasibility constraints). Without order-level information, executed trade data, assumptions, or additional observables, the two functions **are not separately identified**.

State-dependent `H_b,H_a` also changes effective absorption boundaries; a constant shift is not equivalent to a general nonlinear coordinate transformation of the diffusion without accounting for induced dynamics.

### Real-world features and strategy tests

- Queue imbalance `QI_t=(BID_t-ASK_t)/(BID_t+ASK_t)`; track actual depletion and order-book updates.
- Effective spread, microprice and weighted mid; separate realized spread from adverse selection.
- Market buy/sell intensity and cancellations; inferred replenishment; liquidity vacuum after depth withdrawal.
- Event target: next **bid/ask midprice move** versus future return after executable latency; report event time and calendar time.
- Evaluation: reliability bins, Brier, log loss, precision at large imbalance, signed net executed P&L, false signal rates by liquidity state.

### Mechanisms in “Price is a Lie” screenshot

- **Exhaustion:** consuming standing depth makes moving to the next price more likely.
- **Liquidity void:** a sparse book can amplify impact of small market orders.
- **Absorption:** heavy aggressive flow without expected quote movement may signal replenishment; reversal is conditional, not guaranteed.
- **Caveat:** visible quotes are cancellable and information/expectations drive both aggressive and passive liquidity; the post's “price only moves because all orders are consumed” is too absolute.

## 7. Spot–futures lead/lag, physical vs financial barrels

Conference slide states global oil consumption about **100m physical barrels/day** and futures/options **several billion barrel-equivalents/day** with OTC volumes possibly raising the ratio further. Those are presentation figures, not independently audited or net physical deliveries; trading volume counts turnovers of financial exposures, so direct equivalence is misleading.

**Claim to test rather than adopt:** futures can lead spot price discovery; in some settings spot acts like a derivative of liquid futures. Not universal across assets, time and venues.

Candidate crypto features:

$$
\text{basis}_{t,T}=F_{t,T}-S_t,
\quad \text{relbasis}_{t,T}=\frac{F_{t,T}-S_t}{S_t}.
$$

Study funding-adjusted basis, information share, lead/lag regressions, market depth and best execution feasibility. Control timestamp skews, exchange-specific outages, limits and actual tradability. Out-of-sample leads can disappear after data-latency delays.

## 8. Physical inventory squeeze as a constrained diffusion analogy

Slide titled **A Stylized Model of the Squeeze**:

- Physical supply/demand may be unobservable, inventory may be observed.
- Stock-out `x=0` and tank-top `x=1` boundaries encourage reversion.
- Inventory follows a stylized mean-reverting diffusion, `dx=κ(\bar x-x)dt+σdW` (boundary treatment required).
- Spread `f(x,t)=S(x,t)-F(x,t)` depends nonlinearly on inventory, especially near full/empty storage constraints.
- Extreme boundary penalties can induce squeeze-like behavior; the slide sketches a terminal distribution/first-passage representation.

**Distinctions:** continuous diffusion on `[0,1]` requires reflecting/absorbing/regulated boundary conditions; delta-function payoff descriptions are distributional idealizations and must be mathematically justified. Oil storage is not equivalent to crypto market-maker inventory.

**Research analog:** liquidity scarcity near inventory or financing limits can create convex price responses. Test specific measurable proxies before importing the analogy.

## 9. Signal processing: Singular Spectrum Analysis (SSA) of a spread

User's method: model spread as structured low-dimensional components plus transient noise. Useful for relative value, cointegration residuals and microprice/futures spreads if properly causal.

### Correct embedding and reconstruction

Given spread `s_1,...,s_T`, embedding length `L`, `K=T-L+1`:

$$
X=\begin{bmatrix}
s_1&s_2&\cdots&s_K\\
s_2&s_3&\cdots&s_{K+1}\\
\vdots&\vdots&\ddots&\vdots\\
s_L&s_{L+1}&\cdots&s_T
\end{bmatrix}\in\mathbb R^{L\times K}.
$$

Singular-value decomposition:

$$
X=\sum_{j=1}^r \sigma_j u_j v_j^\top.
$$

Choose a set `S` of signal indices and reconstruct

$$
X_{sig}=\sum_{j\in S}\sigma_j u_jv_j^\top,
\qquad \tilde s_t=\operatorname{DiagonalAverage}(X_{sig})_t,
\qquad n_t=s_t-\tilde s_t.
$$

**Correction to supplied rough note:** the singular components use **`u_j v_j^T`**, not `v_j v_j^T` (which would generally have wrong dimensions). Hankelization/diagonal averaging maps the trajectory matrix back to time series.

### Signal and execution logic

Suggested *lagged* Z-score, with baseline built from data strictly before decision time:

$$
z_t=\frac{\tilde s_t-\operatorname{Mean}(\tilde s_{t-W:t-1})}
{\operatorname{Std}(\tilde s_{t-W:t-1})}.
$$

Candidate entry gate:

$$
|z_t|>\tau,\quad |n_t|<\eta,
$$

interpreted as an unusually persistent rather than noise-only spread departure. Exit when `|z_t|≤ε`, when `|n_t|` spikes, or upon time/risk limit.

**Critical:** standard SSA applied to all `T` bars uses future observations to reconstruct **past** values. The operational feature must be recomputed at each `t` using only `[t-Lookback+1,t]`; also inspect whether reconstructed current endpoint is reliable. Use incremental/rolling SSA or past-only forecasts. Test whether residual stationarity/cointegration survives structural breaks. Account for both legs' fees, hedging and non-synchronous fills.

### SSA experiment matrix

`L={20,40,80,160}`, rank/energy grouping, `W={50,100,200}`, `τ={1,1.5,2,2.5}`, noise gate `η` relative to recent residual dispersion, exits `ε={0,0.25,0.5}`, stop/time limits. Every combination is a multiple hypothesis; keep a sealed confirmatory sample and adjust interpretation for selection.

## 10. Concrete alpha strategy families (all [HYPOTHESIS])

### A. Mean reversion

- Detrended rolling log price at `z≤-2`: long; exit around zero or target `+2` only if tested; short mirror where allowed.
- Bollinger `±2..3σ` fade, EMA or trend residual, `0-tangent` local flattening after divergence.
- Thin-volume drop reversal: conditional returns after `-0.3%`, `-0.5%`, `-1%` shocks over 1/5/10/15/30 minutes at low volume; compare momentum continuation.
- Range/flat regime volatility-extreme maker buys/sells; confirm queue fill and subsequent adverse selection.
- SSA residual mean reversion with noise gate.

### B. Drift following / momentum

- MA crossings `5/15/30`, `5/20/40`, RSI/SMA, trend strength and breadth, breakout with confirmation.
- Prefer evidence of persistence, volume and execution capacity; disable/size down in choppy flat periods.
- Use appropriate stop/exit rules and benchmark versus sign-flipped entries.

### C. Gap filling / dislocation

- Opening price gaps in instruments that actually have market closes; crypto is 24/7, so “gap” may mean discrete liquidity void, outages or cross-venue dislocation.
- Basis convergence for matched spot/futures adjusted for carry/funding and constraints.
- Delayed lead–lag cross-asset trade only if reaction occurs *after* executable observation latency.

### D. Volatility / turbulence / jumps

- Volatility-aware limit order placement around expected swings, maker orders, adaptive inventory; tested conditional on fill probability and drawdowns.
- Predict jump size/intensity versus direction separately; focus on `≤0.5%` jumps over 1–30 minute windows only as candidate range.
- Tail-sensitive sizing and preplanned halts in explosive volatility states.

### E. Breakout and structure

- ATH/global/local highs/lows, higher-low trend stages, resistance absorption and failed-breakout reversal.
- Price level push with rising/persistent volume; distinguish genuine aggressive consumption from transient bid/ask cancellation.
- Structural multi-phase transitions; record pivot confirmation lag to avoid repainting.

### F. Pairs / statistical arbitrage

- Two leg prices/spread, hedge ratio with *only historical* estimation, residual stationarity/cointegration and structural-break checks.
- Signal based on spread z-score/SSA/OU/volume response.
- Trades require synchronized fills; hedging risk, borrow/funding and both legs' costs.

### G. Simple stochastic/ML ideas

- Positive/negative return counters, mean returns 1–3, Markov switching/transition probabilities, HMM, event features, random-entry twins and null strategies.
- Strong feature/model tests compared with simple baselines; never promote a feature because an in-sample chart looks smooth.

## 11. Strategy-regime mapping (proposal)

| State | Candidate strategies | Extra safeguards |
|---|---|---|
| Flat, moderate vol | Detrended/SSA and Bollinger fades; passive bid/ask swing | No crossing against strong hidden drift; maker fills must be credible |
| Uptrend, low/mod vol | Trend continuation, breakout, higher low | Guard against trend exhaustion, false breaks |
| Downtrend | Momentum or sell/short where permitted; long exposure reduction | Spot-only constraints; do not assume shorting accessible |
| High vol, deep liquidity | Wider discretionary maker bands, smaller inventory, jump models | Larger stops, capped order size, halt conditions |
| High vol, shallow liquidity | Reduce gross exposure, no-trade or strictly smaller/tighter risk | Stress gap fills/slippage, reserve capital |
| Uncertain/conflicting state | Flat, small exploratory book, diversified strategy mixes | Explicit confidence calibration; avoid forced trades |

**Benchmark experiment:** evaluate each strategy in unconditional samples *and* each model-predicted regime at decision time. If a strategy improves only when conditioned on labels assigned afterward, it is not an implementable result.

## 12. How to prove (or falsify) signal value

Define entry/exit event, fixed horizon and offset to executable price. For each feature/strategy, report:

- Conditional future-return distribution, median/mean, tail quantiles and bootstrap CIs.
- Calibration of event probabilities, class imbalance, temporal clustering and cluster-robust uncertainty.
- Gross and net bps per trade, total net P&L, turnover, average hold time, max drawdown, tail losses and concurrent inventory usage.
- Breakdown by asset, date, regime, liquidity, hour/session, signal value and volatility.
- **Cost crossover**: the maximal spread+fee+slippage under which estimated edge remains positive.
- Delayed-entry tests: `+1`, `+2`, `+5` bars (or realistic milliseconds for book signals), preventing latency fantasy.
- Held-out periods, independent venues and newly available data after research freeze.
- Negative controls: shuffled event timestamps (respect seasonality), opposite side, random entries, naive trade on next bar and buy-and-hold.

**Correct decision rule:** do not place a trade based solely on statistically significant return forecast. Require expected risk-adjusted executable value to exceed costs *with uncertainty margin* and portfolio/risk constraints.
