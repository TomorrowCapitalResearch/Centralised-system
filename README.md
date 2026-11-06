# Quantitative Research & Systematic Trading — Master Knowledge Archive

**Snapshot:** 2026-11-06  
**Status:** Research notes / design hypotheses / references — **not** a verified trading system or a claim of profitable alpha.  
**Source scope:** This conversation's screenshot batches, accompanying discussion and user-supplied working notes. This repository-ready archive deliberately preserves unfinished ideas as hypotheses instead of silently presenting them as results.

## Where to start

| File | Contents |
|---|---|
| `01_MATHEMATICAL_FOUNDATIONS_AND_RESEARCH_METHOD.md` | Scientific philosophy, assumptions, stochastic calculus/SPDEs, rough paths, stationary action, Fourier, graphs, information theory, Wasserstein flows, sampling and identification. |
| `02_MARKET_STATES_MICROSTRUCTURE_AND_STRATEGY_RESEARCH.md` | Data, feature engineering, volatility, regime inference, queue depletion, hidden liquidity, SSA spread signals, spot/futures, oil inventory analogy and concrete strategy hypotheses. |
| `03_PORTFOLIO_CONTROL_EXECUTION_OPTIMISATION_AND_VALIDATION.md` | Bellman control, allocations, inventory, P&L attribution, no-free-lunch optimisation, simplex, execution, backtesting, validation, reporting and screenshot performance evidence. |
| `04_MODELS_ARCHITECTURE_MACRO_RESOURCES_AND_CAREER.md` | ML methods, neural/representation models, Qlib/Kronos/TradingAgents/WorldMonitor, macro seminar notes, professional-role patterns, firms, grants and references. |
| `05_RESEARCH_REGISTER_ROADMAP_AND_MASTER_PROMPT.md` | Prioritized reproducible experiments, hypotheses, acceptance/rejection criteria, schedule, master research prompt and templates. |
| `research_register.csv` | Machine-sortable hypothesis register. Import into GitHub, Excel, Pandas or project-management software. |

These files are independent Markdown notes with substantial internal structure. The `.csv` is a compact index, not a substitute for the explanatory notes.

## Core objective

Build a modular, causally correct and reproducible quantitative research framework connecting:

```text
Raw market / macro / cross-asset / order-book data
  -> point-in-time quality checks, timestamps and normalization
  -> features (price, volatility, volume, microstructure, networks, factors)
  -> market-state posterior with calibrated uncertainty
  -> candidate alphas + conditional strategy selection
  -> portfolio/inventory/risk optimization
  -> order execution simulation and live controls
  -> P&L attribution, proper benchmarks and out-of-sample validation
  -> hypothesis registry, negative-result archive and iteration
```

**Goal:** discover *reproducibly positive expected net returns after realistic costs*, not merely attractive price plots, model metrics or in-sample backtests. No claimed return, Sharpe or win rate in an image should be treated as established without source code, dates, costs, independent testing and point-in-time data.

## Evidence legend — apply to every note

- **[MATH]** established identity/result *under explicitly stated assumptions*.
- **[PAPER]** research result attributed to a specific paper; scope may be narrow.
- **[SCREENSHOT]** claim or reported empirical finding seen in an image; not independently audited.
- **[HYPOTHESIS]** proposed interpretation or trading effect requiring falsification.
- **[IMPLEMENT]** useful engineering concept or architectural suggestion.
- **[UNVERIFIED]** unverified numbers, links, promotional outcomes, institutional claims, or incomplete transcriptions.
- **[REJECTED]** failed/ruled-out proposition to preserve in the negative-result archive (no such status is assigned merely because a screenshot seems weak).

A source citation in these notes is *provenance*, not proof that its claim is correct. The files add some external canonical links checked on 2026-11-06; all other screenshot-derived descriptions remain grounded only in the supplied images.

## Reading the research with the correct attitude

> Quants should be skeptical about their assumptions and optimistic enough about inquiry to test them. Pessimism is a methodological safeguard, not an identity.

- Start with a precise question. Enumerate assumptions, failure modes, observables and alternative explanations.
- Separate algebraic facts, statistical findings, causal interpretations and a profitable executable trade.
- Avoid dogma in **both** directions: do not accept a model because it is prestigious; do not reject useful models because the institution is prestigious.
- Be curious; remain willing to discover that a beautiful idea is economically irrelevant.
- Data volume is not information; complexity is not superiority; backtested profitability is not live profitability.
- Preserve **negative results**. They permanently narrow the search space and stop repeated effort.

## Reference types and limits

1. **Provided images**: LinkedIn posts, mathematical slides, conference photos, a textbook index, advertisements, Q-variance challenge and sample performance charts. No original datasets or full papers were supplied for most images.
2. **User-supplied handwritten/working text**: SSA recipe, Bellman objective, optimizers, institutional-role aspirations, repo links, firm list and research-prompt procedure.
3. **Verified external starting points** (checked, not audited for predictive merit):
   - [WorldMonitor](https://github.com/koala73/worldmonitor)
   - [Kronos](https://github.com/shiyu-coder/Kronos)
   - [TradingAgents](https://github.com/TauricResearch/TradingAgents)
   - [Microsoft Qlib](https://github.com/microsoft/qlib)
   - [BlackJAX Sampling Book](https://github.com/blackjax-devs/sampling-book)
   - [Q-variance challenge](https://github.com/q-variance/challenge)
   - [Chandra–Feltes–Weber parabolic Anderson work](https://arxiv.org/abs/2402.05544)
   - [Bach–Huiberts simplex smoothed analysis](https://arxiv.org/abs/2504.04197)

## Repository hygiene

- Store large raw datasets **outside Git**; keep manifests, checksums, acquisition scripts and schemas inside Git. Never commit exchange API keys, client data or proprietary data.
- Data partitions: `venue/asset/frequency/year/month`; store UTC timestamps, contract/pair identifiers and provenance.
- Freeze experiment configs and seeds, software versions and split dates.
- Include a plain-language summary next to every code result. Never merge research and production code on an unreviewed path.
- Record whether strategies are feasible on **spot only**, or require **margin, borrow, perpetuals or options**.

## Open problems to solve rather than assume away

1. Do we have real order-book messages and trade prints, or only OHLCV? Hidden liquidity, queue position and maker fills are not identifiable from candles alone.
2. Which assets, venues, fees, spreads and funding constraints actually apply during the intended live trading interval?
3. Are regimes descriptive **at time t** or genuinely predictive of **t+h**?
4. Are all signal calculations streaming/causal (no future-centered filters, revised data or forward-looking extrema)?
5. What incremental edge survives fees, spread, slippage, maker non-fills, adverse selection and market impact?
6. Can experiments beat naive references (buy-and-hold, random entry, sign flip, moving-average and volatility baselines)?
7. Will a single consistent portfolio-and-execution accounting system reproduce results across research notebooks and live operations?

## Navigation and terminology

- **Alpha**: expected return not explained by chosen benchmark/factors, which must exceed implementation costs.
- **Regime**: an operational description/distribution of market conditions, not a deterministic oracle.
- **Inventory**: portfolio holdings and associated market/financing risks; exchange queues are *order-book inventory*, a separate variable.
- **Volatility**: conditional dispersion, measured at a specified frequency and annualization convention.
- **Stress test**: sensitivity to unfavorable data, assumptions and mechanics, not just a longer backtest.
- **All results in this archive are proposed or externally reported until independently reproduced**.

## Screenshot intake crosswalk — complete conversation archive

This provenance table groups the screenshot topics by **upload batch in this conversation**, including duplicates. It is not an assertion that every cropped claim was independently verified.

| Batch | Image topics (in upload order) | Where documented |
|---|---|---|
| 1 | Generalised parabolic Anderson SPDE; quant research culture/institutional criticisms; framework boundary; negative results; cross-disciplinary analogy; existence theorems; optimizer No Free Lunch; order-book diffusion/hidden liquidity; branched rough differential equations; liquidity exhaustion | 01, 02, 03, 04 |
| 2 | ARCH/GARCH families; 6/20/60 whiteboard; Quantreo feature list; stochastic control books; periodic table of ML; ML cheatsheet; neural-network free-energy/spectral post; actuarial index; quant firm promotional tier list; LinkedIn risk-management profile | 01, 02, 03, 04 |
| 3 | Smoothed simplex method; LQG 2025 seminar calendar; “mega forces” macro regime; AI employment exposure; lower real Sharpe projection; changing stock–bond correlation; U.S. exceptionalism; purchasing-power versus volatility risk; public debt/CBO charts; smart-beta fees | 03, 04 |
| 4 | Qlib platform; financial versus physical oil derivatives; graph Laplacian; oil inventory squeeze; Financial Information Theory; Python/C/Rust benchmark; speculative alternative gravity narrative; q-variance challenge; BlackJAX Sampling Book; option market-making SDE | 01, 02, 03, 04 |
| 5 | Queue-depletion screenshot again; path-integral stationary action; Factor Uncertainty Index; Fourier diagram; near-closed-form implied volatility; “seven quant lessons”; graph-network strategy; alpha-potential games; Wasserstein heat gradient flow; five-model Sharpe performance table | 01, 02, 03, 04 |
| 6 | Q-variance duplicate; BlackJAX duplicate; second-order Langevin; market-making SDE duplicate; QF-Lib example backtest; Q-variance duplicate; deterministic/stochastic/dynamic programming; fifteen trading strategies; founder-grant list; satirical quant-firm specialties | 01, 02, 03, 04, 05 |

One screenshot of claims about public/private math and the speculative physics image are intentionally kept separate from validated mathematical results. Repetition within batch 6 does not constitute independent corroboration.
