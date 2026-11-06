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
