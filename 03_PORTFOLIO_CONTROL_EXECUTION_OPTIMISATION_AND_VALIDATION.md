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
