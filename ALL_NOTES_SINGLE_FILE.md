# Complete Research Compendium — Single-File Edition

This concatenates the five detailed note sheets for single-page GitHub reading. The five split documents are canonical; edit them first, then regenerate the single-file edition.

# 01 — Mathematical Foundations, Model Assumptions & Research Method

> **Purpose:** capture the mathematics referenced across the screenshots, what each theory actually guarantees, what it does *not* guarantee, and how it might lead to falsifiable research in finance, market microstructure, learning or physics.

**Related files:** [Market states and microstructure](02_MARKET_STATES_MICROSTRUCTURE_AND_STRATEGY_RESEARCH.md) · [Portfolio/control/validation](03_PORTFOLIO_CONTROL_EXECUTION_OPTIMISATION_AND_VALIDATION.md) · [Experiment register](05_RESEARCH_REGISTER_ROADMAP_AND_MASTER_PROMPT.md)

## 1. Scientific working philosophy

### Skepticism without paralysis

User's original formulation, edited for clarity: quantitative researchers should be pessimistic about unsupported claims, guided by mathematical reasoning, and aware that mathematics itself is riddled with assumptions. Identify those assumptions. Stay curious. Avoid dogmatism. Too much distrust can cause a person to shut out learning, collaboration, and useful contradictory evidence. Pessimism is a *guide to method*, not a personality trait.

A practical workflow:

1. Express the claim unambiguously: what variables, horizon, units, state and trade action?
2. Write assumptions that make the derivation meaningful. Which assumptions are observable or testable?
3. Derive mathematical consequences, including limiting and pathological cases.
4. Separate a model that fits historical observations from a useful causal or probabilistic model.
5. Construct the cheapest decisive counterexample and null benchmark.
6. Test on discovery data; freeze design; confirm on unseen data.
7. Report negative findings with the same care as positive findings.
8. Revise beliefs instead of defending prior work.

### Research claims must be separated

- **Mathematical theorem:** an implication of hypotheses; not automatically a description of real markets.
- **Statistical pattern:** an observable regularity with uncertainty; not necessarily causal or stationary.
- **Economic mechanism:** a proposed causal explanation; not necessarily profitable.
- **Trading rule:** a policy that can be executed after constraints and costs; not necessarily profitable even if predictive.
- **Validated strategy:** positive, robust net economic performance on leakage-controlled independent data; still may decay live.

### Limits and negative results

Negative results worth archiving: proofs of non-identifiability; structural impossibility theorems; signals fully explained by spread/costs; models that only work after future data has leaked; performance limited to one date/asset or narrow parameter value; cases where higher model complexity does not improve a simple baseline.

The images argue research incentives may undervalue negative findings, cross-disciplinary analogy and non-constructive existence proofs. These are **methodological prompts, not evidence for claims that private institutions possess undisclosed decades-ahead mathematics**.

## 2. Basic stochastic process notation

For an Itô diffusion,

$$
 dX_t=b(t,X_t)\,dt+\sigma(t,X_t)\,dW_t,
$$

where drift describes conditional mean infinitesimal change and diffusion describes stochastic variation under an assumed probability model. With jumps, introduce a compensated or ordinary jump measure depending on the decomposition desired. For market prices, specify measurement noise, discreteness, stops, time changes, volatility and whether state is observed/latent.

Distinguish:
- discrete bar returns versus instantaneous continuous-time returns;
- realized historical variance versus conditional forecast variance;
- predictable drift versus observed finite-sample trend;
- risk-neutral probabilities used for pricing versus physical probabilities used for trading;
- existence, uniqueness and numerical stability versus statistical identification and calibration.

### Fundamental existence and identifiability questions

1. Does a solution exist and is it unique under the stated coefficients/noise?
2. Are moments finite and does the solution explode?
3. Is the discretization convergent at the desired sampling rate?
4. Can parameters be identified from available data?
5. Are estimated parameters stable across assets, years and regimes?
6. Does inference have sufficient effective observations after serial dependence?

## 3. Parabolic Anderson SPDE and regularity structures [PAPER]

**Reference:** Ajay Chandra, Guilherme de Lima Feltes and Hendrik Weber, *A priori bounds for 2-d generalised Parabolic Anderson Model* / published version *A priori bounds for the generalised parabolic Anderson model*, [arXiv:2402.05544](https://arxiv.org/abs/2402.05544), published in *Communications on Pure and Applied Mathematics* (2026).

Representative SPDE:

$$
(\partial_t-\Delta)u=\sigma(u)\xi.
$$

Here `u` is a stochastic field, `Δ` a spatial Laplacian, `σ` a state-dependent coefficient, and `ξ` an irregular driving distribution (not an ordinary smooth function). Classical multiplication may be ill-defined. Regularity structures lift such objects into a mathematically controlled setting.

The work establishes a priori estimates implying **non-explosion and polynomial-in-time growth** and global well-posedness for the specified 2-dimensional generalised parabolic Anderson model on a torus under its regularity/lifting assumptions, with other SPDE applications. **It does not prove that any proposed rough-volatility or infinite-factor financial model is stable.**

**Research questions:** If using an SPDE for option surfaces, stochastic volatility fields or latent factor curves, specify space/time domain, noise distribution, functional space, boundary conditions and relevant existence/uniqueness bounds *before* trusting large-scale simulation.

## 4. Rough path theory, path signatures, neural differential equations

### Why paths matter

Order flow, returns, volume, volatility and liquidity are observed as paths. A path's ordered geometry may encode information lost in averages, histograms, correlation coefficients and even sets of fixed lags.

A controlled differential equation is conceptually

$$
 dY_t=V(Y_t)\,dX_t.
$$

When `X` is too irregular for classical Riemann–Stieltjes integration, **rough path theory** adds iterated-integral information that can define a solution under suitable assumptions.

A path signature begins

$$
S(X)_{s,t}=\left(1,\int_s^t dX,\int_{s<u_1<u_2<t} dX_{u_1}\otimes dX_{u_2},\ldots\right).
$$

- Higher-order terms capture interactions and ordering among channels.
- Requires careful treatment of normalization, truncation order and computation cost.
- Not automatically predictive; sometimes ordinary lag features are equally good.

### Branched neural rough differential equations (B-NRDEs) [SCREENSHOT]

The ICML 2026 social post depicts a log-ODE solver for a branched rough differential equation: lift input control to a branched rough path, take log-signature in an appropriate Hopf algebra, combine vector fields, integrate a local ODE to update latent state.

A pictured expression (notation adapted) is

$$
L_{s,t}=\sum_{p\in\mathcal B^{\mathrm{prim}}_{\mathcal H}}\langle p,\lambda_{s,t}\rangle F_{\mathcal W}(p).
$$

**Candidate experiment:** identical input features and causal splits for (a) linear regularized model, (b) boosted trees, (c) TCN/LSTM, (d) neural CDE/RDE or signatures. Compare predictiveness *after* transaction costs, not just training loss.

**Correction:** neural ODEs and rough paths are related but not “the same object”; a neural ODE normally uses regular time dynamics and a rough differential equation handles a richer irregular driver.

## 5. Feynman path integrals and stationary action

The screenshot describes a quantum transition amplitude

$$
\mathcal A(B\leftarrow A)=\int_{q(A)=A}^{q(B)=B}\mathcal Dq\,e^{iS[q]/\hbar}.
$$

Paths contribute complex phases according to the action. Near stationary-action trajectories, nearby phases can interfere constructively; away from stationary action, highly oscillatory contributions may cancel in suitable approximations. Observable probability is related to the squared modulus of an appropriate amplitude.

**Important:** this is quantum-mechanical theory, not evidence that prices obey the Feynman path integral or follow a mechanically optimal trajectory. A financial “action” requires an explicit definition of state, units, constraints, physical/statistical interpretation and an independent empirical prediction.

**Candidate research (exploratory only):** define a functional for a financial path, e.g. penalizing acceleration, variance, deviation from some price/volume path, or execution cost; compare its minima and residual with simpler smoothing/optimal-control baselines. Avoid imposing future endpoints when using it to trade.

## 6. Fourier, spectral and periodic-signal analysis

For an integrable signal, the Fourier transform decomposes frequency content:

$$
\hat x(\omega)=\int x(t)e^{-i\omega t}\,dt.
$$

Possible market uses: intraday seasonality, volume cycles, oscillatory mean-reversion, spectral coherence, cross-spectral lead–lag, filtering, detection of changing dominant periods and non-periodic noise.

**Do not infer a stable tradable cycle from a large periodogram peak alone.** Financial series are nonstationary and heteroskedastic; spectral leakage, aliasing, multiple testing and sample-window choice are central risks.

Procedure: transform log returns or defensibly detrended level, compare with surrogate series preserving volatility clustering, use rolling or multi-taper estimates, avoid centered/zero-phase filters in live simulations, verify forward predictability.

Additional candidate methods: STFT, wavelets, Hilbert transforms, SSA, autocorrelation, seasonal regressions and spectral entropy. Each has trade-offs in localization and causality.

## 7. Stationarity, persistence and structural features

- **ADF test:** null of a unit root, conditional on regression lag/deterministic terms; rejection is not proof of all kinds of stationarity.
- **KPSS:** null of a selected form of stationarity; pair with ADF with care.
- **Hurst exponent:** a scaling statistic whose estimate is sensitive to method and nonstationarity. `H<0.5` is not sufficient proof of profitable mean reversion.
- **Autocorrelation:** linear temporal dependence; robust inference needed under heteroskedasticity and overlapping returns.
- **ARCH LM test:** tests for autoregressive conditional heteroskedasticity in residuals.
- **Skewness, excess kurtosis:** marginal distribution shape, sensitive to outliers and finite samples.

Price levels, log prices, returns, spread residuals and volume are different mathematical objects. Test each separately. Detrending with the full sample uses future information and cannot be used directly as a causal trading feature.

## 8. Graphs, diffusion and the graph Laplacian [MATH]

Given a symmetric nonnegative weighted adjacency matrix `A` and diagonal degree matrix `D` with `D_ii=Σ_j A_ij`, the graph Laplacian is

$$
L=D-A,\qquad (Lf)_i=\sum_j A_{ij}(f_i-f_j).
$$

Masses connected by springs give a mechanical picture: restoring force `-Lf` pulls adjacent displacements toward one another. Graph heat dynamics are

$$
\frac{df}{dt}=-Lf,\qquad f(t)=e^{-tL}f(0).
$$

For positive symmetric weights, `f^T L f = (1/2)Σ_ij A_ij(f_i-f_j)^2 ≥0`. A real financial graph may be directed, signed or time-varying, requiring alternate Laplacians and stability checks.

**Market applications:** asset graph from correlations, partial correlations, normalized mutual information, lead–lag causality candidates or actual liquidity connections; spectral gaps/community structure; diffusion of cross-asset shocks; central vs peripheral allocations; topological change-point features. Raw correlation is not causal transmission.

## 9. Information theory for market regimes

**Paper seen in screenshots:** Miquel Noguer i Alonso, *Financial Information Theory* (dated Nov 20, 2025). The screenshot claims applications to regime detection, efficiency diagnostics, entropy-adjusted VaR, diversification and S&P 500 data from 2000–2025. Treat comparative performance claims as [SCREENSHOT] until independently reproduced.

Entropy:

$$
H(X)=-\sum_x p(x)\log p(x).
$$

KL divergence (not symmetric):

$$
D_{KL}(P\Vert Q)=\sum_x p(x)\log\frac{p(x)}{q(x)}.
$$

Mutual information:

$$
I(X;Y)=\sum_{x,y}p(x,y)\log\frac{p(x,y)}{p(x)p(y)}.
$$

Transfer entropy (schematic directional conditional MI):

$$
TE_{X\to Y}=I(Y_{t+1};X_t^{(k)}\mid Y_t^{(l)}).
$$

- **MI** detects nonlinear dependence, not causal direction.
- **Normalized MI** is definition-dependent: state your normalization and estimator.
- **Transfer entropy** is directional predictive dependence conditional on chosen history, not evidence of causality unless assumptions exclude confounding.
- Discretization, kernel/binning, nearest-neighbour estimation, lookback and bias corrections matter especially for high-frequency data.

**Proposed experiments:** crypto return entropies across horizons; MI/conditional MI among BTC, ETH and alts; transfer entropy and price response; rolling KL/Jensen–Shannon divergence to a historic regime distribution; incremental forecasting gain after volume, trend and volatility controls.

## 10. Wasserstein optimal transport, entropy gradient flow

Wasserstein-2 distance for probability measures `μ,ν` with finite second moments:

$$
W_2^2(\mu,\nu)=\inf_{\pi\in\Pi(\mu,\nu)}\int\|x-y\|^2\,d\pi(x,y).
$$

Entropy functional (relative to an appropriate reference measure) is

$$
\mathcal F[\rho]=\int\rho\log\rho\,dx.
$$

Under suitable conditions, **gradient flow** of entropy in Wasserstein space produces the heat equation

$$
\partial_t\rho=\Delta\rho.
$$

This is a deep mathematical link between transport, diffusion, optimization and entropy, not evidence of stationary asset prices.

**Experiments:** distance between rolling return/volume factor distributions and a reference; optimal transport for regime transition diagnostics; compare with simpler KS, energy distance, Cramér–von Mises, covariance shifts and MMD. Use identical estimation windows, sample sizes and backtest splits.

## 11. Langevin dynamics, posterior sampling, stochastic gradients

The second-order/underdamped Langevin SDE shown is

$$
\begin{cases}
 dX_t=V_t\,dt,\\
 dV_t=-\gamma V_t\,dt-\nabla U(X_t)\,dt+\sqrt{2\gamma/\beta}\,dW_t.
\end{cases}
$$

Velocity can help explore correlated targets; friction, mass matrices, temperature, step size, discretization bias and multimodality determine behavior. A sketch can show trajectories, but does not establish mixing correctness.

The screenshot's **BlackJAX Sampling Book** lists: contour stochastic-gradient Langevin dynamics, cyclical SGLD, Pathfinder, periodic orbital MCMC, tempered sequential Monte Carlo, inner-kernel tuning for SMC, comparisons of SMC and persistent sampling, microcanonical Langevin Monte Carlo, analysis of MCLMC runs, change of variables in HMC, elliptical slice sampling for Gaussian regression and Bayesian regression with latent Gaussian sampling.

Canonical resource: [blackjax-devs/sampling-book](https://github.com/blackjax-devs/sampling-book).

**Applications:** Bayesian volatility/state model parameter posterior, posterior predictive uncertainty, robust portfolio weights integrating posterior uncertainty, change-point models. Distinguish calibrated posterior uncertainty from sampling algorithm numerical error. Run convergence diagnostics, multiple chains, effective sample size, R-hat, predictive checks.

## 12. Neural network weight spectra and statistical mechanics

The screenshot attributed to Charles H. Martin proposes layer-quality metrics through a derivative of a generating/free-energy-like quantity and a Harish-Chandra–Itzykson–Zuber integral. Exact paper metadata and interpretation are **not independently verified here**; avoid overinterpreting the cropped equation.

Useful general questions: whether Hessian/weight singular-value spectra, stable rank, heavy-tail exponents or spectral norms predict generalization beyond in-sample loss. Compare to sharpness-aware and simple validation-based methods, controlling for architecture and layer scale.

**Avoid:** asserting a direct analogy between statistical-mechanical phase transitions and observed market regime transitions without deriving observables and falsifiable predictions.

## 13. Existence proofs, construction, and cross-field analogy

- Brouwer fixed-point theorem, Nash embedding and similar results establish existence under mathematical hypotheses; they do **not** guarantee efficient computation or usable finite-data estimators.
- Non-constructive results can motivate approximate numerical algorithms, but complexity, conditioning and practical constraints decide usability.
- Analogy map: rough paths ↔ irregular financial signals; first-passage theory ↔ queue depletion; statistical mechanics ↔ interacting agents; control theory ↔ inventory/portfolio; filters ↔ hidden state; graph theory ↔ asset networks; optimal transport ↔ distribution shifts.
- Be careful with historical anecdotes (e.g. “Einstein used geometry already available for 180 years”). Geometry predating general relativity is true in broad outline, but the slide's precise time-span is not itself a validated historical claim.

### General framework boundaries

Black–Scholes assumes a particular market model. Smile/skew findings motivate stochastic volatility, local volatility and jump extensions. Rough volatility is a further modeling family; none is an all-purpose replacement. State **which** observed feature or pricing objective each model addresses and compare fit versus implementation burden.

## 14. Alternative gravity and cosmology materials [SEPARATE / SPECULATIVE]

A screenshot contrasted Pythagoras, Newton, Lagrange, Einstein, Noether and an alternative “WaveCode” description of structured space and conserved tension. Treat purported contradictions such as “GR breaks global symmetries,” “energy is not conserved,” or “string theory is necessary” as **simplified or contested claims**, not established physics.

Technical distinction: in general relativity local covariant conservation `∇_μ T^{μν}=0` holds, but a global scalar energy can fail to be canonically defined in generic spacetimes lacking time-translation symmetry. The issue does not refute GR. Noether's theorem connects continuous symmetries to conserved currents in systems with the relevant action symmetries.

Keep any alternative cosmic-expansion/dark-matter hypothesis in a *separate scientific repository* with explicit equations, conserved observables, predictions against ΛCDM/GR and datasets. Do not contaminate the financial backtest with metaphorical physics.

## 15. Research protocol in one page

**For every proposition**: `ID | Question | mechanism | assumption list | math | observable data | controls | test | predicted sign/effect size | null | confirmatory split | result | limitations | status | next test`.

**For every data transform**: provide code, timestamps, estimator definition, window length, boundary handling, scaling, NaN policy, look-ahead audit.

**For every numerical model**: theory checks, synthetic tests with known ground truth, calibration uncertainty, repeatability and failure modes.

**For every strategy**: explicit signal/entry/exit/position/fees/slippage/latency/fill rules; net P&L; multiple comparisons correction; out-of-sample holdout; stress; trade/no-trade baseline; negative result archive.


---

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


---

# 03 — Portfolio Allocation, Dynamic Control, Execution, Optimisation & Validation

> **Objective:** move from a collection of alpha candidates to a capital-constrained, risk-aware and reproducible strategy ensemble with faithful trading economics. All performance claims cited from screenshots are **unverified** unless source data are recreated.

## 1. Decision making: Bellman recursion vs a one-step signal

User's reference:

$$
V(s)=\max_a\{R(s,a)+\gamma V(s')\}.
$$

This is intuitive **when the next state `s'` is deterministic**. In a stochastic Markov decision process the expectation or transition probabilities are required:

$$
\boxed{V(s)=\max_{a\in\mathcal A(s)}\left[\mathbb E(R(s,a,S')\mid s,a)+\gamma\mathbb E(V(S')\mid s,a)\right]}
$$

or `V(s)=max_a[R(s,a)+γ Σ_{s'}P(s'|s,a)V(s')]` for deterministic conditional immediate reward `R`.

**Interpretation:** maximize the value of the action's immediate consequence **plus discounted continuation value**, under uncertainty and constraints, not “buy if the next predicted bar rises.” For finite horizon, add time index `V_t(s)` and terminal utility `V_T(s)`.

### Candidate trading state and action

$$
s_t=(q_t,\,cash_t,\,\widehat r_t,\,\widehat\Sigma_t,\,z_t,\,spread_t,\,depth_t,\,funding_t,\,\text{regime}_t).
$$

Actions: target inventory, aggressiveness/taker-vs-maker, cancel/replace, hedging, instrument selection, stop/flat and risk limits. Rewards = realized change in marked-to-market wealth *minus* execution/funding/capital costs and risk penalty.

**Crucial distinction:** planning in a simulated market requires transition and fill models. An inaccurate fill model can make a mathematically optimal policy lose money live.

## 2. Stochastic optimal control

From references to **Controlled Markov Processes and Viscosity Solutions** and René Carmona's **Lectures on BSDEs, Stochastic Control, and Stochastic Differential Games with Financial Applications**:

$$
dX_t=b(t,X_t,\alpha_t)dt+\sigma(t,X_t,\alpha_t)dW_t.
$$

Choose admissible controls `α` minimizing

$$
J(\alpha)=\mathbb E\left[\int_0^T f(t,X_t,\alpha_t)\,dt+g(X_T)\right],
\quad V(t,x)=\inf_\alpha J_{t,x}(\alpha).
$$

Under regularity assumptions a Hamilton–Jacobi–Bellman equation describes the value function; viscosity-solution theory permits some nonsmooth cases. Financial portfolios with discrete trades, state constraints, jumps and costs are often not directly solvable by a simple smooth PDE.

**Engineering ladder:** fixed heuristic -> constrained static optimizer -> limited-horizon model predictive control -> stochastic scenario tree / dynamic programming -> reinforcement learning only after model and evaluation controls mature.

## 3. The portfolio manager is the final decision layer

**Strategy-level forecasting** should generate candidate returns and uncertainty. **Portfolio-level allocation** decides the book after correlations, capital limits, turnover, inventory, exposure, cost and borrow constraints.

A risk-adjusted, turnover-penalized model:

$$
\max_{w_t}\quad
w_t^\top \hat\mu_t -\frac{\lambda_t}{2}w_t^\top\hat\Sigma_t w_t
- C(w_t,w_{t-1})
$$

subject to valid leverage, gross/net exposure, concentration, liquidity, availability and margin constraints. For a robust version optimize worst-case or posterior-averaged returns/covariance; for a downside-sensitive version use CVaR or drawdown surrogate. `\hat μ,\hat Σ` are forecasts with estimation error, not observed constants.

### Portfolio objectives

- Capital growth/net return; volatility and drawdown budgets.
- Diversification/correlation to existing holdings and external market exposures.
- Transaction count, market impact, capacity and execution delay.
- Directional exposure, BTC beta, market regime and downside crash exposure.
- Cash reserves and collateral; maximum simultaneous orders and live position limits.
- Weighted utility of action *plus continuation value*, including replacement opportunities.

**Multi-objective optimization:** do not collapse all objectives into a vague score without declaring units and weights. Use Pareto fronts, epsilon-constraint formulations, robust scenarios and clearly specified priorities.

### Position sizing

Candidates: fixed fraction, inverse-vol, volatility targeting, risk parity, forecast-weighted risk budgets, Kelly fraction with shrinkage, drawdown-aware caps and liquidity-capacity sizing. For estimation-uncertain probabilities and returns, full Kelly is often far too aggressive. Keep max exposure and per-asset limits independent of forecasts.

### Factor investing and asset allocation

The user's notes suggest emulating institutional roles: AQR-style factor decomposition, systematic allocation, Bridgewater-like scenario analysis, Renaissance-like signal discovery, Jane Street/Citadel market mechanics and rigorous optimization. These are **aspirational research standards, not knowledge of proprietary models or practices**.

Work from beta to residual alpha: market, size, value, momentum, quality, carry, low-volatility, sector/geography, currency and crypto-specific factors. Regress P&L on known exposures; factor-hedged performance may be much smaller than a naive equity curve.

Reference idea: *From Satellite to Core: Integrating Quantitative Strategies into Total Portfolio Allocation*, attributed in user notes to **Aous Labbane, Investcorp**. Bibliographic details and original content need checking; archive as [UNVERIFIED REFERENCE]. Question: when can one strategy become a core allocation rather than satellite without creating concentrated factor/tail exposure?

## 4. Stochastic programming versus dynamic programming

Screenshot contrasts deterministic optimization, stochastic programming/scenario trees and Bellman recursion.

- **Deterministic LP/QP:** optimizes a scenario or fixed expectation with explicit constraints.
- **Two-stage stochastic programming:** choose here-and-now decision, then recourse after uncertainty is revealed.
- **Multistage stochastic programming:** sequence of non-anticipating decisions on a scenario tree.
- **Dynamic programming:** represent future effects through a state-value function / transition kernel; computational burden is often the curse of dimensionality.

They are different formulations that overlap in mathematical substance, not unrelated universes. Non-anticipativity is fundamental: a scenario-tree policy cannot choose actions based on future branch information unavailable at the time.

A screenshot comment claims most portfolio optimization can be done by LP. Many important constrained formulations can indeed be linearized (e.g. linear costs, simple absolute deviation); covariance risk, nonlinear impact, options and nonconvex constraints can require quadratic, conic, mixed-integer or general stochastic optimization. Choose simplest valid representation.

## 5. Market-making, inventory and P&L attribution

The option-MM screenshot represents marked-to-market book wealth `V_t` as:

$$
dV_t\approx dV_t^{\rm trade\ jumps}
+dV_t^{\rm volatility\ carry}
+dV_t^{\rm volatility\ shocks},
$$

with execution jumps integrated against bid/ask point processes, an option vega times variance-risk-premium drift, and Brownian changes from volatility shocks. The original has a portfolio summation over contracts and quote sizes.

**Meaning:** every income source should be assigned to spread capture, carrying inventory/exposures or stochastic repricing. Options additionally require Greeks (delta, gamma, vega, theta), volatility skew and hedging P&L. This exact option SDE is not a ready-made spot crypto MM model.

### Spot trading P&L ledger (consistent book)

$$
\begin{aligned}
\Delta W_t &\equiv W_{t+1}-W_t\\
&=\underbrace{q_t\,(M_{t+1}-M_t)}_{\text{inventory mark-to-market}}\\
&\quad+\underbrace{\text{execution edge vs reference mid}}_{\text{spread / price improvement}}\\
&\quad-\underbrace{\text{fees + funding + slippage + impact}}_{\text{cash costs}}.
\end{aligned}
$$

Choose an explicit mark and time convention; **spread capture and adverse selection can overlap with inventory price movement if defined loosely**. Define realized spread after `h`, effective spread at execution, permanent/transient impact and adverse selection consistently to avoid double counting.

### Execution economics

The user's **working fee assumption for Revolut X**: maker `0%`, taker `0.09%` (9 basis points) per side. **Treat fees as a configuration to verify from the actual account/venue before a live decision**. Two taker sides imply ~18 bps in nominal trading fees alone, excluding spread, impact and funding. A maker fill may be adverse-selected and a missed maker order can forfeit alpha.

For a long trade, illustrative decision:

$$
\mathbb E[\text{gross price return}\mid\mathcal F_t]
>\mathbb E[\text{roundtrip fees+spread+impact+slippage+opportunity cost}]
+\text{required uncertainty margin}.
$$

### Order modeling

- Market order: cross spread, taker fee, slippage, depth and price impact.
- Limit order: fill only if valid historical trade/book flow crosses available queue volume; exposure to non-fills and adverse selection.
- Order state machine: NEW, ACK, PARTIAL, FILLED, CANCEL_PENDING, CANCELED, REJECTED, EXPIRED, UNKNOWN/RECONCILE.
- Reconcile actual open orders/trades from venue with internal state, including disconnections.
- Kill switches: stale price, stale book, exceeded daily loss, risk-limit breach, excessive slippage, connectivity failure, unexpected position, high error rate, data integrity failure.
- Execution constraints: rate limits, API permissions, minimum order size, notional limits, rounding, funding, venue operating/legal restrictions.

## 6. No Free Lunch in optimisation

Peter Cotton screenshot compares 11 optimizers across diverse smooth, multimodal, noisy, control, high-dimensional and constrained benchmarks. Rankings differ by problem; no algorithm dominates **all** problems. The classical No Free Lunch theorem only guarantees averaged equality under specified distributions over objective functions; real tasks have structure that good optimizers exploit.

### Named benchmark suite from the original figure / user notes

| Benchmark | Characterization in source | Relevance to trading research |
|---|---|---|
| Rosenbrock | smooth curved valley | ill-conditioning / tuning |
| Rastrigin | multimodal | local optima |
| Ackley | flat/bumpy | broad basins and deceptive flatness |
| Lens (`LENIS` in typed note) | rugged/needle-like | sparse good solutions; spelling verify |
| Tetris | noisy | stochastic function evaluations |
| Plinko | noisy | random outcome objective |
| Creature | emergent | complex feedback |
| Genetic art | high-dimensional | black-box parameter dimension |
| Antenna | multimodal | multimodal engineering design |
| Ebola | control | dynamic intervention optimization |
| Robot arm | constrained | constraints/costs |
| Brachistochrone | smooth | trajectory optimization |
| Rocket | control | nonlinear dynamic control |
| Battery | arbitrage | time-dependent storage decision |

**Optimizers to evaluate:** PRIMA/BOBYQA (`PRIMA_BOBYOA` in rough notes is a typo), Differential Evolution, Particle Swarm, CMA-ES, Harmony Search, Random Search, Genetic Algorithm, Nelder–Mead, Powell, Simulated Annealing, Evolution Strategies, plus Bayesian optimization and gradient-based optimizers where appropriate.

Do not claim the heatmap's colored ranks prove a universal best method. Save actual function evaluations, wall-clock, best score, robust median/worst quantile, variance over seeds, constraint violations and budget fairness. Different evaluation budgets are a common source of misleading rankings.

### Wind Rose and Flood Fill ideas [USER HYPOTHESIS]

User's shorthand mentions **mono- and multi-parameter objective optimization using “Wind Rose” and “Flood Fill.”** No formal definitions were provided. Possible operationalization:

- **Wind Rose:** visualize objective dependence on parameter *direction* and distance around candidate optimum (polar slices, directional sensitivity, anisotropy).
- **Flood Fill:** identify contiguous regions of parameter space whose performance clears a minimum threshold, and measure basin width, connectivity and stability instead of selecting just the sharp global peak.

These are working interpretations; keep their definitions provisional until the original intended algorithms are specified. Strong strategies should occupy **broad stable regions**, not one tiny parameter point produced by optimization noise.

### Smoothed simplex method [PAPER]

The Quanta screenshot discusses work by **Eleon Bach and Sophie Huiberts** on simplex analysis; source [Optimal Smoothed Analysis of the Simplex Method](https://arxiv.org/abs/2504.04197). Classical worst-case simplex can be exponential, while smoothed analysis studies randomized perturbations and explains improved practical behavior under specified assumptions. It does not say every LP is easy, nor that LP is automatically the ideal trading optimizer.

## 7. Backtesting infrastructure, architecture, testing

The Qlib diagram separates:

```text
Static research workflow:
 Data server -> feature/data enhancement -> predictive models -> ensemble
 -> portfolio generator -> order executor

Dynamic modeling:
 model creators + model manager; ensemble and portfolio/executor creators

Analysis & feedback:
 alpha analyzer + portfolio analyzer + execution analyzer
 -> research feedback, reports and changed configurations
```

The design has strong parallels to intended modular framework. Use one clean interface per layer to prevent ad-hoc notebooks from each defining their own P&L and risk semantics.

### Proposed repo package structure

```text
src/quantlab/
  data/{schemas,ingest,quality,point_in_time,cache}.py
  features/{returns,volume,volatility,microstructure,spectral,graph,information}.py
  states/{baseline,hmm,filter,calibration}.py
  alphas/{api,registry,mean_reversion,momentum,breakout,spread,jumps}.py
  models/{baselines,ml,training,uncertainty}.py
  portfolio/{constraints,allocator,inventory,risk_budget}.py
  execution/{orders,fills,fees,simulator,venue_adapter,reconcile}.py
  backtest/{event_clock,engine,results,walk_forward}.py
  evaluate/{metrics,attribution,stress,reports,stats}.py
  research/{experiment_registry,config,seed,provenance}.py
notebooks/{00_data,01_features,02_state,03_alpha,04_portfolio,05_execution,06_audit}/
tests/{unit,integration,causality,property,benchmark}/
configs/  docs/  reports/  references/
```

### Minimal strategy interface

One import + decorator idea:

```python
from quantlab.alphas import signal

@signal(name="rolling_z_fade", required_fields=["close"])
def rolling_z_fade(frame, cfg):
    # return timestamp-indexed desired exposure or target-order intent
    # features only from available data up to the decision timestamp
    ...
```

Need precise contracts for: signal time, fill reference, target position (vs action), confidence, allowed venue/side, data lookback, warmup, NaNs, position sizing and costs.

**Previously encountered implementation risk** in discussions: notebook evaluation calling undefined `lab` object or demo setup. Provide instantiated `ResearchLab` or explicit inputs; no implicit notebook state. Add `pytest` smoke tests on a tiny synthetic dataset.

### Suggested required tests

- **Causality test:** modifying future rows must not change historical signals or past decisions.
- **Identical trades test:** independent cash ledger reconciles to engine equity curve.
- **Zero-friction controls:** constant price, randomized signals, no trades, sign-flip, 0 fees, known synthetic return stream.
- **Limit fill conservation:** filled quantity ≤ available trade-through quantity respecting queue assumptions.
- **Regime lookahead control:** label computed at `t` must not use final state after `t`.
- **Data invariants:** monotonic timestamps, OHLC inequalities, positive prices, no impossible negative physical inventory.
- **Restart/resume:** consistent order state and cash/positions after restart.
- **Walk-forward engine:** sealed hold-out must not be called by model selection.

## 8. Validation: reported screenshot metrics versus proof

A performance screenshot included five models (exact visible report values transcribed):

| Model | Sharpe | Return | Final wealth | vs B&H | Max DD | Win/Loss | Profit factor |
|---|---:|---:|---:|---:|---:|---:|---:|
| Model 1 | 2.94 | 112.3% | $14.51 | 5.1x | 8.2% | 1.57 | 2.41 |
| Model 2 | 2.58 | 113.7% | $12.64 | 4.5x | 9.1% | 1.34 | 2.18 |
| Model 3 | 2.97 | 156.2% | $8.77 | 3.1x | 11.4% | 1.33 | 2.52 |
| Model 4 | 2.95 | 104.8% | $11.97 | 4.2x | 7.8% | 1.59 | 2.35 |
| Model 5 | 3.03 | 109.8% | $10.35 | 3.7x | 8.5% | 1.69 | 2.47 |
| Buy & Hold | 1.01 | 35.0% | $2.83 | 1.0x | 18.6% | 1.01 | 1.12 |

**Status [SCREENSHOT/UNVERIFIED].** Missing: dates, capital normalization, leverage, universes, out-of-sample selection, strategy descriptions, whether wealth and return are on comparable bases, gross/net costs, rebalance and execution mechanics. Do not infer that the Sharpe ratios are achievable.

Another image depicts a **QF-Lib example strategy** with equity curve, monthly/yearly returns, month-by-month heatmap, return histogram, QQ plot and rolling 6-month return/volatility. Use its **reporting format**, not its displayed success as validation.

### Mandatory strategy scorecard

| Component | Metrics / evidence |
|---|---|
| Return | annualized/total net return, CAGR where meaningful, alpha vs benchmark |
| Risk | maximum drawdown and duration, volatility, tail losses, CVaR, exposure |
| Risk-adjusted | Sharpe with correct frequency/serial correlation adjustments, Sortino, Calmar |
| Trade economics | gross/net bps, spread paid/earned, maker fills, slippage, funding, partial fills |
| Stability | asset/year/regime splits, rolling metrics, bootstrapped CI, parameter regions |
| Model fit | calibration and uncertainty, probabilistic score, feature ablation, drift |
| Scientific control | independent test, sign-flip, random entry, naive benchmark, multiple testing |
| Operational | throughput, stale data rate, order errors, latency, venue limits, uptime |

Crypto Sharpe annualization is often mishandled when trade P&L, per-minute returns, overlapping holdings or daily equity returns are mixed. Calculate from a consistently sampled **portfolio equity return** series. Account for autocorrelation and a realistic financing/risk-free convention; show exact frequency and formula.

### Split and multiple-testing discipline

- Chronological training/validation/test; purged/embargoed CV if labels overlap.
- Discovery -> freeze model/parameters -> one or more sealed confirmatory periods -> genuinely fresh paper/live.
- Nested validation for model selection, never retune after examining sealed confirmation.
- Reality check for researcher degrees of freedom; track number of tested ideas, not just survivors.
- Reject leakage from revisions, future event labels, global normalization, centered smoothing, look-ahead pivot detection, delayed publications and survivorship selection.
- Stress cost/latency shocks, market closures/outages, crash regimes, unfilled maker orders, moving correlations and partial fill uncertainty.

### Distinguish prediction from strategy

A `55%` positive next-move probability can still be unprofitable if average adverse move/fees dominate. A `45%` win-rate strategy can be profitable if reward/risk is favorable and execution real. Win rate, Sharpe and high gross returns are never sufficient alone.

## 8A. Full strategy taxonomy from the “15 Trading Strategies” screenshot

The promotional chart mixes **alpha strategies**, **execution algorithms** and **model-search tools**. Preserve all 15 while making their actual roles explicit:

| Screenshot category | What it is in a research architecture | Warning/test |
|---|---|---|
| 1. Pairs trading | relative-value/stat-arb alpha hypothesis | correlation alone is not cointegration; two-leg execution |
| 2. Scalping | short-horizon directional/spread trading style | capacity and latency/cost sensitive |
| 3. Smart order routing | execution optimization across venues | does not independently create a forecast edge |
| 4. Market making | liquidity provision/inventory strategy | adverse selection and queue risk |
| 5. Momentum | trend/continuation alpha | regime reversal and turnover |
| 6. TWAP | time-sliced execution | compare to arrival price and implementation shortfall |
| 7. VWAP | volume-profiled execution / performance benchmark | needs unbiased volume forecasts |
| 8. Seasonality | calendar/session alpha hypothesis | many tests and time-zone alignment |
| 9. Volatility trading | variance/volatility exposure strategy | requires specific option/futures/hedge mechanics |
| 10. ML-based strategy | model family for forecast or policy | data snooping and out-of-sample validation |
| 11. Pattern recognition | technical-rule alpha hypothesis | labels can repaint; protect against hindsight |
| 12. Sentiment analysis | event/text feature or signal | source availability timestamp and language bias |
| 13. Deep RL | sequential policy-learning method | simulator reward hacking and fill realism |
| 14. Adaptive strategies | parameter/regime-dependent policy family | adaptation must use only historical information |
| 15. Genetic algorithms | strategy/parameter search method | massive search multiplicity and backtest overfitting |

**Rule:** Every named strategy needs a specific tradable action/exit/risk specification and an edge beyond alternative implementation costs. A sophisticated strategy label is not itself evidence.

### Implied-volatility inversion: numerical robustness note

A LinkedIn post attributes a near-closed-form inverse-implied-volatility method to **Wolfgang Schadner**. It claims to replace Newton/Brent root loops with an expression using the inverse normal CDF; its example mentions a roughly **1.7×** speedup and fewer deep OTM / 0DTE missing values. **These are unverified single-user benchmark claims**, not a validated global accuracy or numerical-stability result.

Research test: assemble a grid of strikes, expiries, prices, forward/discount conventions, both calls/puts and extreme moneyness. Compare against accurate high-precision reference root solvers, record no-solution conditions (e.g. arbitrage-violating quote), maximum volatility error, runtime, monotonicity and stability near intrinsic value. Don't replace an inversion in production solely on a social post.

## 9. Operational risk and stress playbook

1. **Before deployment:** dry-run with real-time data and simulated orders; verify venue fee schedule, limits, API and symbol availability.
2. **Live canary:** minimal exposure, hard caps, deterministic rollback, independent portfolio reconciler.
3. **Daily:** reconcile orders, cash, positions, mark prices, realized/ex-ante risk; check unexpected fills.
4. **Weekly:** compare realized to expected fill rate, alpha decay, state-model calibration and net attribution.
5. **Monthly:** report stress scenarios, turnover, gross/net exposures, benchmark/factor attribution, live-versus-backtest drift.
6. **Failure response:** stop trading on data gap, ledger mismatch, broken quote, venue outage, invalid fills or risk exceedance; do not optimize on the fly during a crisis.

## 10. Working order of implementation

**First:** data correctness, canonical return/cash ledger, fees and baseline reporting. **Second:** simple causal signals and state features. **Third:** realistic portfolio/execution simulation. **Fourth:** conditional allocation by regime and inventory state. **Fifth:** advanced stochastic control, posterior sampling and RL only with proven baseline benefit. This ordering prevents sophistication from hiding incorrect fundamentals.


---

# 04 — ML Models, Software Architecture, Macro Investing, References, Firms & Funding

> **Purpose:** preserve the less directly executable but strategically valuable materials in the screenshots: machine-learning model families, public codebases, research methods, macroeconomics, economics of investing, quantitative finance career targets, seminars and independent-project funding.

## 1. Machine-learning algorithms — map of tools and failure modes

One screenshot gave a “machine-learning cheatsheet.” A more useful research classification for finance is below. Always compare against simple baselines; higher prediction accuracy need not mean higher net P&L.

| Family | Methods | Useful for | Common weaknesses |
|---|---|---|---|
| Linear regression | OLS, ridge, lasso, elastic net | Forecasts and interpretable factor exposure | nonstationarity, misspecification, multicollinearity |
| Logistic / probabilistic classification | Logit, probit, calibrated classifiers | directional event probabilities | calibration, temporal leakage, rare classes |
| Tree ensembles | Random forest, ExtraTrees, XGBoost/LightGBM/GBDT | nonlinear cross-feature effects | overfit on regime changes and noisy labels |
| Support-vector machine | linear/kernel SVM | medium-size event classification | scaling, cost, calibration and feature choice |
| KNN | nearest neighbours | analog historical states | curse of dimensionality and speed |
| Naive Bayes | conditional feature independence approximation | inexpensive classification baseline | miscalibration with strongly correlated features |
| Clustering | k-means, hierarchical, DBSCAN/HDBSCAN | unsupervised market regime exploration | unstable label meaning and metric dependence |
| PCA/SVD | variance compression | factor exposure/covariance/noise | may ignore low-variance predictive signals |
| MLP | general nonlinear mapping | tabular nonlinear conditional models | training data needs, poor causal handling |
| CNN/TCN | local temporal patterns | multichannel market sequences | receptive-field/horizon leakage, regime shift |
| RNN/LSTM/GRU | recurrent state models | streaming sequential dependencies | model selection, long-term optimization, slow training |
| Transformer | attention sequence mapping | long/multimodal data | expensive, data-intensive, can overfit |
| Autoencoder/VAE | representation learning | latent factors, anomalies | reconstruction is not alpha |
| HMM/Markov switching | latent discrete states | regime segmentation and posterior transitions | state labels may be unstable |
| GP/Bayesian regression | uncertainty-aware supervised modeling | uncertainty-sensitive forecasts | scaling, kernel misspecification |
| RL / control | Q-learning, actor-critic, MPC | sequential inventory/portfolio control | simulator bias and objective hacking |
| Evolutionary algorithms | GA, evolution strategy, genetic programming | black-box feature/rule search | severe search overfitting, heavy compute |

**Preferred experimental order:** regularized linear forecast + transparent volatility baseline -> gradient boosting -> temporal ML -> deep representation models -> RL/agents only when incremental net performance is evident.

## 2. Embedding and representation learning — “periodic table” of ML

Mark Hamilton's ICLR 2025 post depicts losses unifying dimensionality reduction, clustering, self-supervised learning, CLIP-style multimodal contrast and supervised learning:

$$
\mathcal L=\int D_{KL}\big(p(j\mid i)\Vert q(j\mid i)\big)\,di.
$$

The organizing axes are **learned-representation distribution** and **supervision signal**. Listed examples include PCA, SNE/t-SNE, Student-t/SNE variations, K-means, InfoNCE/SimCLR, CLIP, SupCon, cross-entropy and others. These are not literally the same optimization problem in all details; the figure maps a selected family of distribution-matching interpretations.

**Use for crypto:** encode windows of returns/volume/book features into a representation that preserves economically meaningful similarities, cluster to tentative regimes, then test whether clusters forecast actual future distribution differences or differing strategy payoffs. Avoid a clustering objective that bakes in hindsight labels.

### Other model ideas preserved from screenshots

- **Rough-path / branched neural RDEs:** higher-order information about irregular time series; see mathematical sheet.
- **Physics of NN weights:** eigenvalue spectra/random matrix theory and generating function/free energy; do not claim validation without full paper.
- **State-space inference:** Kalman variants, particle filtering, nonlinear hidden states and Bayesian posterior sampling.
- **Hybrid architectures:** TCN + LSTM/GRU + static tabular heads might be compared at equal information/compute budget; do not presume larger is better.
- **Graph neural networks:** optional later extension of weighted asset networks; compare to manually calculated centrality/Laplacian features first.

## 3. Public projects to study — verified repository identifiers

### 3.1 [koala73/worldmonitor](https://github.com/koala73/worldmonitor)

**Role:** real-time global situational intelligence with news/geopolitics, finance/commodities, infrastructure and cross-stream monitoring. Public docs also describe specialized site variants, SDK/API/CLI and interactive dashboards.

**Useful parts:** source ingestion architecture, asynchronous event stream normalization, UI for global risk events, event provenance, news summarization. This is **not** a validated causal macro forecasting signal and may include delayed or derived information. Avoid using current news feeds to reconstruct historical knowledge without publication-timestamp archive.

### 3.2 [shiyu-coder/Kronos](https://github.com/shiyu-coder/Kronos)

**Role:** financial-market foundation model focused on market sequences. Its repo describes finetuning tokenizer and predictor stages; an A-share/Qlib example demonstrates preprocessing and a simple top-K backtest.

**Engineering warning from the project:** demonstration backtest is not a production trading system; risk factor neutralization, portfolio optimization, position sizing and high-fidelity transaction-cost models remain necessary.

**Experiment:** compare pretrained/finetuned Kronos feature/prediction against regression, GBDT and TCN with identical past-only timestamps, asset selection and forecast horizon. Evaluate inference latency and net economic returns, not only forecast loss.

### 3.3 [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents)

**Role:** LLM multi-agent financial research framework. Agent teams specialize in technical/fundamental/sentiment/news analysis, bullish/bearish critique, trade proposal, risk and portfolio approval. Project has backtesting and portfolio-context features.

**Use:** research-orchestration pattern (analysts -> adversarial critique -> decision -> risk review -> logged outcome). **Do not treat agent debate as independent evidence or historical trading skill.** LLMs can share correlated errors, hallucinate citations, and read future information unless point-in-time access is controlled. Use deterministic market engine as source of trade truth.

### 3.4 [microsoft/qlib](https://github.com/microsoft/qlib)

**Role:** AI-oriented quantitative investment platform with separable data, feature, model, portfolio, execution and analysis modules. One PyQuant screenshot shows the data server / model / ensemble / portfolio / executor / analyzers and feedback workflow.

**Use:** conceptual architecture, experiment and result configuration conventions, feature/model swapability. Evaluate code compatibility with intended assets/frequencies before integrating.

### 3.5 [blackjax-devs/sampling-book](https://github.com/blackjax-devs/sampling-book)

**Role:** tutorial library for advanced samplers, especially MCMC/SMC, HMC, Langevin families and diagnostics. Read for posterior inference and stochastic volatility calibration, not as plug-and-play alpha.

### 3.6 [q-variance/challenge](https://github.com/q-variance/challenge)

**Role:** reproduce the empirical conditional realized-variance parabolic shape with a parsimonious continuous-time stochastic process. This is a **scientific modeling challenge**, not proof of a profitable strategy.

### 3.7 Other named tools and references

- QF-Lib: screenshot shows report with equity, rolling metrics, QQ plot and annual/monthly heatmaps. Confirm actual library version and availability before installation.
- Quantreo post: module examples `candle_information`, `compute_spread`, `price_distribution`, `kama_market_regime`, `derivatives`, `log_pct`, `auto_corr`, `hurst`, `skewness`, `kurtosis`, `adf_test`, `arch_test`, `sma`, `kama`, `linear_slope`, `close_to_close_volatility`, `parkinson_volatility`, `rogers_satchell_volatility`, `yang_zhang_volatility`.
- Oxford-Man/LQG/CQF and academic talks: education and source-discovery paths, not performance validation.
- David Orrell, *A Quantum of Variance, and the Challenge for Finance*, **Wilmott** 2025(138), DOI [10.54946/wilm.12155](https://doi.org/10.54946/wilm.12155).
- Bach & Huiberts, *Optimal Smoothed Analysis of the Simplex Method*, [arXiv:2504.04197](https://arxiv.org/abs/2504.04197).
- Chandra, de Lima Feltes & Weber, [arXiv:2402.05544](https://arxiv.org/abs/2402.05544), 2-d generalised parabolic Anderson model bounds; see math sheet.
- Carmona, **Lectures on BSDEs, Stochastic Control, and Stochastic Differential Games with Financial Applications**; Bäuerle/Rieder or Fleming/Soner texts for controlled Markov processes (book cover screenshot does not show authors; verify bibliographic metadata before formal citation).

## 4. Macro investing notes from LQG/Bloomberg/AllianceBernstein seminar photographs

Conference slide heading: **“Instability: Debt, Inflation & AI's Impact on Investing”**. Slides show **Inigo Fraser Jenkins**, AllianceBernstein, and future LQG seminar calendar for Nov/Dec 2025 (now past). The photos are an **archival snapshot of the presenters' arguments**, not current verified forecasts.

### 4.1 “Mega forces imply a new investment regime” [PRESENTATION CLAIM]

Four displayed drivers:

1. **Deglobalization** — fragmented labour, supply chains and constraints to growth.
2. **Public debt** — higher interest/fiscal burden potentially makes inflation a policy attraction.
3. **Demographics** — changing working-age populations and support ratios may reduce growth.
4. **Energy transition** — long capital spending/reallocation, commodity needs and supply constraints may raise inflation uncertainty.

The slide's stated bottom line: **higher equilibrium inflation and lower real growth**. Treat this as a macro scenario, not a deterministic conclusion or proof.

**Possible state features:** inflation breakevens, term premium/yields, debt servicing ratios, demographic dependency trends, energy commodity spreads, fiscal impulse, global trade shares, supply-chain disruption indicators.

### 4.2 AI automation, employment and distribution

One slide asks why employment survives 200 years of automation. Optimist case: technology creates new tasks/demand. Pessimist case: the median living standard and distribution of gains may deteriorate; poses UBI/corporate profit share questions.

Bar chart shows **potential automation exposure by sector** compared with **unionization** (source notes as of June 29, 2023). Illustrative exposure values visible on slide: Financial activities ~51, professional/business services ~45, information/media ~34.5, wholesale/retail ~34, healthcare and utilities ~27, manufacturing ~26, food/arts ~25, education/transport ~23, mining ~20, construction ~15, agriculture ~8 (units and measurement methodology require source paper). The statement “jobs most at risk are non-unionized” is an interpretation requiring labor-market nuance.

**Research implications:** wage shares, labor substitution/complementarity, productivity, job transitions, consumer demand, sector relative value, AI capex cycle; avoid naive mechanical “AI exposure -> stock return” forecasts.

### 4.3 “A painful epiphany: expect lower real Sharpe ratios”

Presentation shows asset classes plotted by historical real return and risk and challenges the persistence of historic U.S. equity/stock–bond favorable returns. Possible response options cited: factor risk, illiquid assets, active management and leverage. **Higher nominal Sharpe or leverage is not a free lunch**, and illiquidity can conceal rather than eliminate volatility.

In research, forecast forward real returns conservatively; stress lower factor premia and trading capacity. No guarantee next decade resembles a selected historical benchmark.

### 4.4 “The diversifying power of bonds likely to reduce”

Rolling ten-year stock–bond return correlation charts for U.S./U.K. show significant historical variation, with later parts of sample near/below zero. Slide challenges 60/40's permanent status as a passive default.

**Point:** correlation and bond duration depend on inflation regimes, monetary credibility and shocks. Re-estimate dynamically; assess rate/inflation tail scenarios and bond-equity drawdown co-movement. An observed negative correlation in the latest plotted interval is **not** proof bonds are always useful or useless.

### 4.5 U.S. exceptionalism: structural supports versus vulnerability

**Case for (visible bullets):** relatively favorable demographics, persistent corporate profitability, geographic/security advantages and home market scale, shareholder/corporate-friendly tax/policy environment.

**Case against (visible bullets):** international institutional backlash, dollar weaponization and alternatives, risk of brain drain at universities, weakening of U.S. soft power, possible energy transition leveling and working-age population shocks.

The presentation concludes exceptionalism is **strategic rather than tactical**, not necessarily ending. These are competing narratives, not trade triggers by themselves.

### 4.6 Government debt and fiscal constraints

Slides plot G7 government-debt-to-GDP (GDP-weighted), U.S. debt-to-GDP, projected U.S. interest expense as share of GDP and major CBO expenditure lines (Social Security, healthcare programs, net interest, discretionary and other mandatory). Presenter argues debt ratios resemble historical stress periods in some metrics.

Model channels: rates × debt stock -> interest expenses; growth and inflation affect denominator; fiscal policy and policy reaction determine paths. **Debt/GDP alone is not a sufficient timing model for bonds, FX or equities.** Use issuance, real rates, maturity profile and investor base.

### 4.7 “Smart beta fees: zero might not be a lower bound”

Historical slide charts selected smart-beta ETFs' fees declining sharply, from high early 2010s levels to significantly cheaper offerings by late 2010s/2020.

**Lesson:** systematic public factors can be commoditized; net performance and implementation capacity become more important than backtested gross premia. Negative fees/subsidized management are possible commercial scenarios but not guaranteed.

### 4.8 “What is risk?”

Slide contrasts **volatility as risk** against **real loss of purchasing power**. It lists active management/idiosyncratic alpha, illiquidity, factor risk, more equity exposure, longer equity holding periods and government bonds with lower risk/return, among options discussed.

**Use:** maintain multiple risk definitions—mark-to-market variance, liquidation risk, capital shortfall, inflation-adjusted drawdown, funding/rollover, liquidity and model uncertainty. Any strategy promising lower volatility by hiding price discovery may merely transfer risk.

### 4.9 LQG seminar calendar in slide [HISTORICAL]

- Nov 11, 2025: evening/hybrid talk **Ilia Bouchouev**, *Lock, Stock and Virtual Barrels: Quantitative Trading in the Oil Market*.
- Dec 9 or Dec 16, 2025: annual general meeting and annual debate (the slide gives alternatives, not finalized date).

These dates are past and are historical references, **not upcoming events**.

## 5. Commodity finance — oil futures versus physical barrels

The oil presentation states financial futures/option barrel-equivalents may trade many multiples of physical daily consumption, and argues futures can lead price discovery. Derivatives turnover cannot be read as unique barrels or available supply; gross contracts can offset and trade repeatedly.

**Research connections:** spot/futures basis, storage constraints, convenience yield, inventory squeeze, shocks to funding and hedging demand, lead-lag/price discovery. Crypo spot-perpetual basis is analogous in *some* mechanisms, but funding/collateral and physical storage differ.

## 6. Actuarial/risk references from textbook index

Screenshot's index is a **topic locator, not a statement of findings**:

- Statistical: Shapiro–Wilk normality test, skewed distribution, standard/stochastic error, seemingly unrelated regressions.
- Simulation/inference: Bayesian methods, bootstrap, stochastic methods and practical confidence intervals.
- Reserves: Chain Ladder link-ratio family, Bornhuetter–Ferguson, Schnieper's model, Clark LDF, separation method, smoothing-type models, Merz–Wüthrich method.
- Regulation: Solvency II capital, technical provisions, contract boundaries, Swiss Solvency Test.
- Long horizon: age-period-cohort, discounting and speed of settlement.
- Historical software mentioned: JAGS, WinBUGS and various reserving tools.

Finance transfers: uncertainty aggregation, extreme loss scenarios, longitudinal model risk and governance; reserving formulas do not become trading signals without reinterpretation.

## 7. Professional skill standard: emulate research disciplines, not proprietary knowledge

User's aspirational roles, preserved with practical tasks:

| Inspiration | Training exercise / deliverable |
|---|---|
| Citadel portfolio management | constrained multi-strategy allocation and gross/net risk budget |
| Citadel senior quantitative analyst | production-quality alpha research with leakage, regime and cost controls |
| Two Sigma research | rigorous historical stress, controls, walk-forward/OOS and experiment tracking |
| Bridgewater risk management | scenario/historical stress, economic exposures, portfolio resilience |
| Renaissance Technologies | exhaustive search for narrow statistical edges with multiple-testing defense |
| Goldman Sachs technical strategist | transparent structure, momentum, volatility, price-level and cross-asset technical work |
| Fundamental research (firm unspecified) | cash flows, valuation, balance-sheet, earnings and business risks |
| AQR factor researcher | multi-factor asset pricing, robustness, cross-sectional signals |
| Bridgewater macro analyst | rates, inflation, fiscal, demographics and global liquidity framework |
| Pershing Square analyst | concentrated position thesis, variant perception, downside and catalysts |
| D. E. Shaw options strategist | volatility surface, Greeks, options structures, execution and hedging |

This table is a **practice curriculum**, not a claim of what any firm's internal processes actually are.

## 8. Quant firm landscape — user-supplied watchlist

The conversation included a subjective employer tier image and the user's fuller target list. **Never present meme ranks or tier lists as objective quality/compensation rankings.** Categorization of the names and spellings below may need verification.

### Personal top-interest names as supplied

Renaissance Technologies (“RenTec”), TGS, XTX Markets; HRT (Hudson River Trading), Radix, Citadel/Citadel Securities, Headlands, Quadrature, PDT Partners, D. E. Shaw, Jump, Jane Street.

### Broader tracked names

Two Sigma, IMC, Voleon Group, G-Research, Xanthum [spelling to verify], DRW, Tower Research, Millennium, Point72, Optiver, Five Rings, SIG, Qube Research & Technologies (Qube RT), Virtu, Squarepoint, Ansatz, Aquatic, Vatic Labs, Arrowstreet, Marshall Wace (appearing in rough note as “Marshall W. engr. gate”, confirm intent), Old Mission, GSA Capital, Schonfeld, AQR, Akuna, Maven Securities, Chicago Trading Company (CTC), Voloridge, Quantlab, Man Group, Capital Fund Management (CFM), ExodusPoint, Verition, Balyasny, WorldQuant, Flow Traders, Belvedere, TransMarket, Gelber Group, Valkyrie, Wolverine, Peak6, Scientech, Group One, XR Trading, DV Trading, Teza and Trexquant.

### Satirical firm-specialty post [SCREENSHOT]

Examples in meme: Citadel Securities broad multiasset; Jane Street ETFs/market making; Jump and Tower low-latency; HRT ML/data; SIG options volatility; Five Rings secrecy; Optiver/IMC options HFT; XTX systematic FX/equities/crypto; Headlands niche futures; DRW options/volatility. **It explicitly labels itself “facetious.”** Use only to generate questions for interviews, not as verified firm descriptions.

### Separate promotional tier screenshot [UNVERIFIED]

S: Renaissance/Jane Street/HRT; A: D. E. Shaw/TGS/PDT; B: SIG/IMC and others; C: Two Sigma/Optiver/DRW; D: Belvedere/Flow/Akuna/Valkyrie; F: Fidelity/Bank of America/Royal Bank of Scotland. This is an arbitrary marketing ranking, mixing asset managers, market makers, prop firms and banks.

### Application planning

Compare by role rather than brand: QR vs QD vs PM vs market making vs risk, programming languages, mathematical focus, data ownership, HFT versus midfrequency, UK work authorization requirements, compensation structure, training, and interview process. Document independently verified facts per employer.

## 9. Academic and project funding leads [UNVERIFIED SCREENSHOT LIST]

The screenshot lists program names and **promotional dollar figures**, not confirmed active eligibility as of Nov 2026. Verify current official websites, country/citizenship rules, deadline, for-profit eligibility, equity conditions, application terms and award status before relying on them.

| Program | Amount described in image | Important qualification |
|---|---|---|
| Activate Fellowship | $100k stipend + $100k R&D, equity-free | deep-tech/program eligibility and location to verify |
| 776 Foundation Fellowship | $100k, young climate founders | climate focus, age and current applications to verify |
| Edge City Grants | $5k–$40k | program specific terms |
| Thiel Fellowship | $200k over 2 years, founders ≤22 | age/eligibility need verification |
| Vercel Open Source | $3.6k credits | **credits**, not cash |
| O'Shaughnessy Ventures Fellowship | $100k equity-free | verify exact program terms and cohort |
| Afore Capital Grants | $1k plus mentoring | verify active rounds |
| Google for Startups Cloud | up to $200k credits over 2 yrs | credits with usage restrictions, not grant |
| Astera Residency | $125k–$250k salary + research budget | **salary/residency**, not grant funding |
| 1517 Fund Medici | $1k | verify eligibility |
| Da Vinci Fellowship | claimed $150k equity-free | verify official program name and funding |

**Links supplied in post** (shorteners are not verified official pages): `https://www.776.org/`, `https://buff.ly/iYGayOr`, `https://buff.ly/GSb0yuM`, `https://buff.ly/A7WgDic`, `https://buff.ly/NZvtoIT`, `https://buff.ly/mF6Y4EJ`, `https://buff.ly/cqYr1DN`, `https://buff.ly/Xbddih7`, `https://buff.ly/MpcNSf6`, `https://buff.ly/JMdGzRz`. Use caution with tracking/redirect links.

Funding is separate from strategy validation. A research grant can support experiments but does not imply market alpha or commercial viability.

## 10. Professional profile/communication screenshot

One screenshot supplied a LinkedIn “About” example of a security-risk-management graduate who described education, internship, domain frameworks (NIS2, GDPR, ISO 27001), transferable analytical skills, desired roles and relevant keywords.

**Reusable structure**, not the person's identity or background:

1. One-sentence professional specialization.
2. Credible evidence: degree, relevant projects, measurable outputs, tools.
3. Most directly relevant domain competencies.
4. Value to a specific team, with limitations described truthfully.
5. Concrete roles sought and availability.
6. Relevant search keywords without excessive unrelated hashtags.

Apply to quant applications with mathematical statistics, stochastic processes, algorithmic research, Python/C++, scalable data engineering, risk/portfolio modeling, backtesting and demonstrable research artifacts. Good signaling is a transparent organization of real work, not exaggeration.

## 11. Cross-topic takeaways

1. **Data source and timestamp correctness dominate model sophistication.**
2. **Cross-disciplinary mathematics is a hypothesis generator**: graph Laplacians, optimal transport, random matrices, SPDEs and control theory are useful only with valid financial observables.
3. **Model choice follows the scientific question**: stochastic volatility for uncertainty, spectral features for periodicity, queue processes for order-book mechanics, control for sequential risk.
4. **Macroeconomic scenarios** belong as stress/factor-state inputs, not unconditional 1-minute trading signals.
5. **External repos** are engineering references to evaluate, not a license to copy unverified performance claims.
6. **Career and funding notes** should be indexed but kept conceptually separate from empirical alpha claims.


---

# 05 — Research Register, Prioritization, Roadmap & Reusable Master Prompt

> **Purpose:** turn notes into a functioning GitHub research program. A good idea is not a completed experiment. Each idea below should eventually have a GitHub issue, hypothesis, code artifact, data provenance, expected sign/effect, benchmark, costs, confirmatory test and explicit verdict.

## 1. Status taxonomy

| State | Meaning | Promote only when... |
|---|---|---|
| BACKLOG | idea/source captured | specific observable and objective defined |
| SPECIFIED | math, assumption, data and null fixed | data/code reproducible on small synthetic test |
| IMPLEMENTED | reproducible software and synthetic tests | streaming causality validated |
| DISCOVERY | experimental exploration allowed | model, parameters and tests frozen |
| CONFIRMATION | sealed holdout used once with frozen design | significant, practical net edge survives specified tests |
| PAPER | tiny/live-sim exposure or shadow execution | fill/cost assumptions confirmed against actual venues |
| LIVE_CANDIDATE | positive predeclared criteria and risk approval | minimal exposure, kill switches and reconciliation work |
| LIVE | deployed under controlled risk | audited live outcomes and monitoring |
| NEGATIVE | tested, not supported under studied conditions | retain failure notes and precise scope |
| OPEN_MATH | requires proof/identifiability checks before prediction | theorem or counterexample established |

Do **not** promote a screenshot, paper abstract or author's social-media results directly to CONFIRMATION.

## 2. Prioritized experiments — concrete issue candidates

### Foundation and integrity (P0)

**P0-01 — Canonical market-data loader.** Ingest spot one-minute bars for 5–11 assets; timestamps in UTC, missing intervals, overlaps and provenance. Validation: known synthetic gaps/duplicates detected and raw files checksummed.

**P0-02 — Causal feature interface.** `add_market_features(df)` returns only values observable by end of each bar. Test: randomly alter `df[t+1:]`; all features `≤t` unchanged. Special concern: SSA, global extrema and whole-sample detrending.

**P0-03 — Single accounting ledger.** Every run reconciles cash, inventory, fees, mark-to-market, realized P&L and exposure. Test using hand-calculable buy/hold/sell sequences, zero-price-change, partial fills and sign-flipped strategies.

**P0-04 — Cost/venue matrix.** Confirm actual execution venue fee tiers, available order types/symbols, minimum size, spreads, depth and connectivity; include maker non-fill and adverse selection assumptions in simulator.

**P0-05 — Evaluation report.** Equities, Sharpe, Sortino, PF, net bps/trade, max drawdown/duration, allocation, rolling metrics, QQ plot, yearly/monthly heatmaps, regime and asset attribution, full config and data checksum.

**P0-06 — Splits and contamination audit.** Time splits, embargo/purge for overlap, point-in-time features, frozen confirmation sample; count experiment trials and total tuned hyperparameters.

### Intraday alpha (P1)

**P1-01 — Low-volume drop reversal.** At thresholds `-0.3/-0.5/-1%` over `1/5/10/15/30` minutes, examine forward `1–60` minute returns conditional on volume quantiles; compete against unconditional and high-volume drop continuation. Reject if no net bps at realistic latency/cost.

**P1-02 — Detrended price / EMA / Bollinger fade.** Rolling past-only trend and Z-score. Compare `-2→0`, `-2→+2`, `±2–3σ` entries and 5/20/40 MA cross. Baselines sign flip, hold, random entry; split by trend/flat regimes.

**P1-03 — SSA denoised spread.** Test exact Hankel embedding/SVD/hankelization, past-only rank selection, current-point stability; compare standard raw-spread Z, OU and cointegration residual, with both legs and execution constraints.

**P1-04 — Action-like stationary trajectory signal.** Define any “stationary action” feature precisely and independently; trade on divergence/flattening/return to zero only if causal and predictive, benchmark against moving averages/curvature and randomized placements.

**P1-05 — Structure and breakout.** Confirmed local/global extrema, ATH, rising volume on level push, false-breakout absorption. Label pivots only after confirmation delay. Event and holding-time analysis by volatility/liquidity.

**P1-06 — Volume and US-open regimes.** Price moves by minute relative to US session openings versus controls and other regional sessions; adjust for risk-on macro releases and asset composition.

**P1-07 — Jump and turbulence.** 0.3–0.5% shock clustering, arrival intensities and conditional sign persistence; compare predictive survival to trivial jump-autocorrelation model.

**P1-08 — Spot/futures/basis lead–lag.** Synchronized price series and executable delay; test funding/basis predictability, BTC→alts propagation and cross-venue gaps; reject if reaction happens before orders could be placed.

### Market states and uncertainty (P1/P2)

**P1-09 — Baseline market-state engine.** Direction, volatility, breadth, liquidity and uncertain label; calibration tests, next-state transition probabilities. Compare to rolling vol and sign persistence.

**P1-10 — GARCH family comparison.** EWMA, historical, Parkinson, RS, Yang–Zhang vs ARCH/GARCH/EGARCH/GJR and stochastic-volatility option. Predict log realized variance and realized tail behavior across horizons and assets; proper scoring.

**P2-01 — Q-variance challenge.** Reproduce empirical conditional parabola independently; assess universality across asset classes/time horizons; simulate competing continuous-time models; document whether coefficient is robust.

**P2-02 — Factor uncertainty.** Rolling factor residual covariance, loading stability and forecast dispersion. Does it predict cross-asset drawdowns after controlling for realized volatility?

**P2-03 — Information-theoretic regimes.** Sample-corrected entropy, MI, NMI, transfer entropy, KL/JSD to reference distribution; test state transitions, not just visualization.

**P2-04 — Dynamic graph risk.** Asset network centrality and Laplacian spectral features; predict correlation convergence, breadth/crash risk; compare raw equal-weight correlations.

**P2-05 — Wasserstein distribution shift.** `W_1/W_2` return distribution drift versus KS/MMD/volatility change; target a *future* regime break; bootstrap intervals.

**P2-06 — Spectral / periodic structure.** Spectrogram, autocorrelation, wavelets, SSA, Hurst and spectral entropy; test true predictive horizons against surrogate and randomized seasonality controls.

**P2-07 — Representation models.** PCA -> GBDT/TCN/LSTM -> contrastive latent market representation -> Kronos baseline, equal data/splits. Only advance if signal net economic value improves.

### Microstructure and portfolio control (P1/P2)

**P1-11 — Queue imbalance and depletion.** Implement first-passage toy model and boundary tests; train/calibrate empirical order-book next-move classifier. Measure calibration, fillability, net returns.

**P2-08 — Hidden-liquidity identification.** Identify parameters only with extra observations/assumptions (trades, replenishment, cancellations), show non-identifiability when using one probability statistic.

**P2-09 — Absorption/void test.** Separate high market-order flow absorbed by resting liquidity versus thin-book level-skipping; event studies controlling spread, volatility, depth.

**P1-12 — Inventory and position allocation.** Fixed weight -> vol target -> constrained multi-alpha mean-variance -> CVaR/risk budgets; compare net portfolio return and exposure stability with no alpha selection leakage.

**P2-10 — Dynamic control.** Bellman/MDP or receding-horizon solution for target orders/inventory; simulate with conservative fill model and simple policy benchmarks; no RL until fill model validated.

**P2-11 — Stochastic posterior uncertainty.** BlackJAX/HMC/SMC or simpler Bayesian approximations for uncertainty in volatility/regime; compare calibration and business outcomes versus deterministic filter.

**P2-12 — Option-MM decomposition and implied vol inversion.** Where options data/permissions exist, verify quote-to-IV numerical inversion, Greeks, variance-risk carry and hedge P&L. “Fast near-closed IV” social post requires independent reproduction.

### Wider mathematics/long horizon (P3)

**P3-01 — SPDE existence and numerical stability.** Formalize a genuine financial SPDE; check applicability of regularity assumptions and discretization theorem before simulation claims.

**P3-02 — Rough paths and branched neural RDEs.** Equal-compute predictions vs TCN/LSTM. Test whether iterated-integral features add skill after strong baselines.

**P3-03 — Weight spectrum and model generalization.** Test layer eigenvalue metrics across seeds/model sizes and forward periods; compare normal validation score.

**P3-04 — Multiple-agent game/control.** Test whether alpha-potential game optimization or simple equilibrium approximations provide a real benefit for inventory competition; require a believable multi-agent market simulator.

**P3-05 — Path-integral/action analogy.** Develop a precise action and prediction, examine dimensional consistency and relation to known optimal control; no claim of quantum financial dynamics.

### Non-alpha backlog

**N-01 — Public-resource assessment:** audit code quality, licenses, dependencies, data contracts and timestamp fidelity for Qlib, Kronos, TradingAgents, WorldMonitor, BlackJAX Sampling Book and QF-Lib.

**N-02 — Employment/communication assets:** targeted resume and clean GitHub research proofs for quant research, development, market making and risk paths.

**N-03 — Funding programs:** independently verify official terms for grant/residency programs; separate cash awards, credits, salaries and cohort restrictions.

**N-04 — Macro regime observations:** reproduce seminar debt, inflation, demographics, bond-equity correlation and productivity data, distinguishing forecasts from historical facts.

**N-05 — External results audit:** reproduce sample five-model high-Sharpe screenshot only if original code/data and rules are obtainable; otherwise label illustrative/unverified.

## 3. Measurable pass/fail criteria (precommit before test)

Use a rule with economic and statistical thresholds rather than a vague “seems profitable.” An example candidate acceptance contract:

```yaml
experiment: P1-01
question: Does a low-volume down jump mean-revert after costs?
assets: [BTCUSDT, ETHUSDT, SOLUSDT]
discovery_period: must_be_specified
validation_period: must_be_specified
sealed_period: must_be_specified
signal_time: after_close_of_event_bar
execution: market_or_limit_with_explicit_queue_simulation
fees: per_venue_verified
holding_horizons_bars: [1, 5, 15, 30, 60]
benchmarks: [random_entry, sign_flip, unconditional, momentum_continuation]
primary_metric: net_bp_per_trade
secondary: [max_drawdown, sharpe, fill_rate, costs, turn_over]
confidence: blocked_or_clustered_bootstrap
multiple_tests: log_every_experiment
pass_rule: precommitted_positive_net_edge_plus_robustness
reject_rule: fails_holdout_or_execution_sensitivity
```

The specific positive threshold must be chosen **before** seeing confirmatory performance; do not invent a universal bps cutoff. Use confidence intervals and economically relevant effect size jointly.

## 4. One-week, one-month and long-horizon sequencing

### First week: make the research reliable

- Build common loaders/schemas on a handful of symbols; catch missing bars and look-ahead.
- Make zero-cost and realistic-fee accounting agree with hand calculations.
- Register 3–5 simple signals and implement a single benchmark scorecard.
- Log known-negative/insignificant results as seriously as winners.

### Weeks 2–4: decisive alpha comparisons

- Test basic price/volume features and reversion vs momentum on multiple assets.
- Measure rolling regime-dependent results and whether regimes can be known at decision time.
- Run SSA spread signal with causality and naive spread controls.
- If order-book data exist, test empirical depletion; if not, prioritize candles/volume and avoid claiming hidden-order detection.
- Freeze one confirmatory period and preserve timestamp provenance.

### Months 2–3: portfolio and execution

- Integrate risk budgets, position constraints, cross-strategy netting and inventory controls.
- Build simulated maker/taker order engine and compare with venue paper fills.
- Compare realized volatility forecasts, GARCH family, graph/network features, and information-theoretic state variables.
- Implement automatic nightly/weekly P&L attribution and research-status report.

### Long-term: expand mathematical depth

- Rough paths, continuous-time models, Bayesian posterior inference, nonlinear dynamics, stochastic optimal control, game-theoretic multiagent extensions and rigorous SPDE work—only when a clearly stated limitation of the simpler platform motivates them.

## 5. Hypothesis note template (copy to `research/ideas/ID.md`)

```markdown
# ID — Concise title

Status: BACKLOG
Priority: P1
Created: YYYY-MM-DD
Owner: ...
Source: screenshot / paper / user hypothesis / observation

## Question and economic mechanism
What specific result must be true for an executable edge?

## Mathematical specification
Variables, equations, units, assumptions, boundary cases.

## Data and availability
Symbols, venue, timestamps, update frequency, lookback and legal access.

## Competing explanations and negative controls
Random entry, opposite side, seasonality, price beta, volatility clustering, delays.

## Experimental protocol
Construction, horizon, costs, time splits, controls and test statistic.

## Anticipated disproof
Under what pattern of results should this idea be dropped?

## Results
Discovery / frozen validation / sealed confirmation, all attempts.

## Final assessment
VERIFIED MATH? EMPIRICALLY SUPPORTED? PROFITABLE NET? OPERABLE LIVE?

## Next action
Explicit test, owner, repo path and due date.
```

## 6. Rigorous master research prompt — expanded from user-supplied six phases

The user's notes identify six phases: **plan first**, **parallel cited research**, **force field-level synthesis**, **turn synthesis into data**, **independent verification**, **deliver two forms**. Below is a reusable expanded prompt. This is an authored research procedure, not a claim that every tool supports parallel workers or interactive dashboards.

```text
ROLE
You are a senior quantitative research lead and skeptical methodological reviewer.
Produce an accurate, falsifiable, citation-disciplined review and research plan.
Optimize for correctness, usefulness and reproducibility, not length or persuasion.

BRIEF
TOPIC: {{precise question}}
AUDIENCE: {{e.g. quantitative PM, ML researcher, risk committee}}
DECISION: {{what practical action the reader must choose}}
TIME WINDOW: {{e.g. last 5 years plus foundational results}}
QUANT LENS: {{e.g. stochastic processes, econometrics, microstructure, control}}
DEPTH: {{fast / standard / exhaustive}}
DATA ACCESS: {{specified sources, permissions, publication timestamps}}
OUTPUTS: {{Markdown article + evidence register/CSV; dashboard only if feasible}}

PHASE 1 — PLAN BEFORE SEARCH
Split the topic into 4–8 non-overlapping strands that cover the full question.
For each strand state questions, relevant data, theory, assumptions and deliverables.
Write an initial hypothesis map and explicitly define the exclusion boundaries.

PHASE 2 — GATHER PARALLEL EVIDENCE WITH TRACEABILITY
Research each strand independently when tools/resources permit.
For each source capture:
  [title, author, year, URL/DOI, publication date, study type,
   question, data, methods, assumptions, equations, actual findings,
   reported uncertainty, limitations, replication availability, confidence].
Keep not-opened or inaccessible sources in an UNVERIFIED list.
Never fabricate citations, numeric estimates or source access.
Do not rely solely on abstracts if full claims require methods/appendices.

PHASE 3 — SYNTHESIZE THE FIELD
Separate consensus, disagreements, partial agreement, research gaps,
contradictory evidence, known impossibility results and recent breakthroughs.
For every popular claim distinguish theorem, empirical estimate,
mechanism hypothesis, simulation and sales/marketing assertion.
Explicitly list assumptions that change the conclusion.

PHASE 4 — TURN SYNTHESIS INTO DATA
Create a machine-readable source register and a hypothesis/experiment matrix.
Provide normalized units, time scales, benchmark definitions and uncertainty.
Show a decision table: approach, supporting evidence, caveat, cost,
required data, known failure, minimum follow-up experiment.

PHASE 5 — VERIFY INDEPENDENTLY
Check primary sources and mathematical identities, re-derive central formulae,
reconcile contradictory numbers, inspect tables against text and verify links.
Run a toy implementation, unit check or synthetic simulation when feasible.
Check selection bias, overfitting, point-in-time integrity and cost assumptions.
List what remains unverified rather than concealing it.

PHASE 6 — DELIVER TWO FORMS
A. Plain-language but mathematically honest article, including executive
   conclusion, formulas, evidence, counterarguments and decision rules.
B. Compact, sortable dashboard/data register, if tools support it; otherwise
   CSV/Markdown tables with equivalent factual content.
Include references, assumptions, negative findings, confidence and next actions.

MANDATORY QUANT FINANCE GUARDRAILS
- No claim of trading profitability without net-of-cost OOS evidence.
- No inference of causality from correlation or predictive association alone.
- Keep physical- and risk-neutral probability measures distinct.
- Distinguish a market label computed with hindsight from a live forecast.
- Make every feature causal and every reported performance figure reproducible.
- Prefer the simplest model that answers the question.
- Treat unsupported personal/industry claims as unverified.
```

## 7. Institutional workflow as a research review board

A multi-agent research workflow can be adapted without granting an LLM authority over capital:

1. **Data and provenance analyst** verifies datasets/time stamps.
2. **Mathematical analyst** derives and boundary-checks models.
3. **Alpha analyst** proposes measurable experiments.
4. **Skeptical researcher** designs adversarial baselines and selection corrections.
5. **Execution analyst** estimates venue fees, spread, fills and operational feasibility.
6. **Portfolio/risk analyst** evaluates exposures, capital and tail risk.
7. **Independent audit** checks cash reconciliation, code reproducibility and holdout integrity.
8. **Decision owner** accepts/rejects research; live trading changes require separate risk controls.

This echoes the modularity of Qlib and TradingAgents without claiming that multi-agent discussions themselves provide empirical evidence.

## 8. The compact final principle

A candidate edge becomes defensible when it has: **(i) an explicit hypothesis; (ii) a causal observable at time of trade; (iii) a fair null model; (iv) positive stable net economics under realistic costs; (v) valid portfolio/risk constraints; and (vi) an independent replication.** Keep all six in one research file.
