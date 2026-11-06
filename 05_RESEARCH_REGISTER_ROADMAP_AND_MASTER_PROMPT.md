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
