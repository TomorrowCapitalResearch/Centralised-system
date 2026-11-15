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




id,area,idea,source_level,priority,status,claim_or_hypothesis,minimal_decisive_test,source_reference
M01,Foundations,SPDE global bounds,PAPER,P3,OPEN_MATH,SPDE assumptions and prior bounds; no generic stability claim,Prove narrow well-posedness or numerical consistency,arxiv.org/abs/2402.05544
M02,Foundations,Rough path signatures,SCREENSHOT,P3,BACKLOG,Irregular multichannel path structure may aid predictions,Compare signature/CDE to lag baseline,ICML 2026 screenshot
M03,Foundations,Feynman stationary action analogy,SCREENSHOT,P3,BACKLOG,Defined path action might provide useful feature,Compare to curvature and smoothing; no quantum inference,social screenshot
M04,Foundations,Graph Laplacian,MATH,P2,SPECIFIED,Asset relationships can be represented as weighted graph,Network features versus correlation baseline,graph physical-analogy screenshot
M05,Foundations,Wasserstein entropy gradient flow,MATH,P2,BACKLOG,Distribution shift signal from Wasserstein distance,Compare to KS/MMD/volatility-change,optimal transport screenshot
M06,Foundations,Layer spectral free-energy idea,SCREENSHOT,P3,BACKLOG,NN layer spectral statistics may reflect generalization,Forward validation versus simple selection criteria,Charles H Martin screenshot
M07,Foundations,Fourier spectral decomposition,MATH,P2,BACKLOG,Seasonal/periodic components may have forward predictability,Causal filtering with surrogate null,Fourier screenshot
M08,Foundations,Negative results and impossibility,SCREENSHOT,P0,SPECIFIED,Archiving invalid models saves research time,Adopt NEGATIVE status; preserve counterexamples,methodology screenshots
M09,Foundations,Cross-field analogy transfer,SCREENSHOT,P3,BACKLOG,Validated mathematical analogies can transfer methods,Explicit mapping assumptions and synthetic tests,methodology screenshots
M10,Foundations,Nonconstructive existence versus computability,MATH,P3,SPECIFIED,Existence alone does not give usable algorithm,Track algorithmic complexity/conditioning,methodology screenshots
S01,States,Close-to-close volatility,MATH,P1,BACKLOG,Baseline conditional risk,Compare to realized variance loss,volatility features screenshot
S02,States,ARCH GARCH EGARCH GJR,MATH,P1,BACKLOG,Conditional variance and asymmetry,OOS likelihood and net risk utility,QFI screenshot
S03,States,Parkinson Rogers-Satchell Yang-Zhang,MATH,P1,BACKLOG,Range-based features may improve volatility forecasts,Compare against close-to-close with same horizon,Quantreo screenshot
S04,States,ADF KPSS ARCH-LM Hurst autocorrelation,MATH,P1,BACKLOG,Regime diagnostics and null assumptions,Rolling ADF/KPSS/vol clustering; future skill,Quantreo screenshot
S05,States,Multi-axis market-state engine,HYPOTHESIS,P1,BACKLOG,Direction vol turbulence liquidity as separate axes,Brier/calibration and future-state score,user research objective
S06,States,Factor uncertainty index,SCREENSHOT,P2,BACKLOG,Factor uncertainty may predict risk,Compare with raw volatility controls,Factor Uncertainty screenshot
S07,States,Entropy mutual information transfer entropy,PAPER/SCREENSHOT,P2,BACKLOG,Nonlinear dependence helps state inference,Forward transition prediction with bias corrections,Financial Information Theory screenshot
S08,States,Dynamic asset correlation network,SCREENSHOT,P2,BACKLOG,Network centrality modifies portfolio risk,Asset-year forward drawdown predictions,network selection screenshot
S09,States,Q-variance,PAPER,P2,BACKLOG,Conditional variance approximates parabola,Reproduce across assets and scales,github.com/q-variance/challenge
S10,States,KAMA market regime,SCREENSHOT,P1,BACKLOG,Adaptive trend regime improves strategy gating,Compare simple MAs/vol classes,Quantreo screenshot
S11,States,Macro inflation debt bonds,SCREENSHOT,P2,BACKLOG,Macro states alter correlation and expected returns,Point-in-time multi-asset regime test,LQG/AllianceBernstein 2025 slides
A01,Alpha,Low-volume drop reversal,HYPOTHESIS,P1,BACKLOG,Thin volume shock reverts more than normal,Net bps for 0.3 0.5 1% shocks,user hypothesis
A02,Alpha,Detrended 2-sigma fade,HYPOTHESIS,P1,BACKLOG,Demeaned residual reverts,Past-only z-score plus signed baseline,user hypothesis
A03,Alpha,Bollinger and EMA fade,HYPOTHESIS,P1,BACKLOG,Deviation from moving center fades,Compare EMA baseline and costs,user hypothesis
A04,Alpha,Moving-average momentum,HYPOTHESIS,P1,BACKLOG,Crossovers forecast continuation,5/15/30 versus 5/20/40 and buy-hold,user hypothesis
A05,Alpha,Jump clustering and postjump return,HYPOTHESIS,P1,BACKLOG,Events predict future sign/tail,Event-study sign/delay controls,user hypothesis
A06,Alpha,US opening windows,HYPOTHESIS,P1,BACKLOG,1-30m US-open effects,Conditional return and execution model,user hypothesis
A07,Alpha,Global local extrema and phases,HYPOTHESIS,P1,BACKLOG,Confirmed structural pivots predict conditional moves,Causal pivot timestamps and benchmark,user hypothesis
A08,Alpha,Breakout and absorption,HYPOTHESIS,P1,BACKLOG,Liquidity absorption and level pushes matter,Book-based event outcomes,liquidity screenshot
A09,Alpha,Pairs cointegration and OU,HYPOTHESIS,P1,BACKLOG,Residual spread reverts,Sealed tests and two-leg costs,user hypothesis
A10,Alpha,SSA spread denoising and noise gate,HYPOTHESIS,P1,BACKLOG,Denoised spread persists more than residual noise,Causal rolling SSA vs raw spread,user supplied derivation
A11,Alpha,Gap filling and spot futures,HYPOTHESIS,P1,BACKLOG,Dislocations converge after execution cost,Basis/funding/latency tests,user notes plus oil screenshot
A12,Alpha,Volatility extreme maker swing,HYPOTHESIS,P1,BACKLOG,Place maker orders around predicted price extremes,Fill and adverse selection cost simulation,user hypothesis
A13,Alpha,Stationary-action-inspired strategy,HYPOTHESIS,P1,BACKLOG,Action-like feature flattening predicts reversion,Feature definition and null signal twins,user hypothesis
A14,Alpha,Mean return and sign counters,HYPOTHESIS,P1,BACKLOG,Simple counters forecast return,Regress conditional returns net cost,user hypothesis
A15,Alpha,Two/three-horizon 6-20-60,SCREENSHOT,P1,BACKLOG,Short indicator relative to longer indicates entry,Confirm exact formula with causal test,whiteboard screenshot
A16,Alpha,Lead lag BTC-to-alts,HYPOTHESIS,P1,BACKLOG,BTC leads alt price moves,Time alignment and latency controls,user research objective
B01,Microstructure,Queue diffusion hitting probability,MATH/SCREENSHOT,P1,BACKLOG,Queue imbalance predicts next quote movement,First passage toy test plus data calibration,queue PDE screenshot
B02,Microstructure,Hidden liquidity identifiability,MATH,P2,OPEN_MATH,One equation cannot identify two hidden queues,Counterexample; add observable restrictions,queue PDE screenshot
B03,Microstructure,Liquidity absorption vs void,HYPOTHESIS,P2,BACKLOG,Aggressive flow versus depth predicts continuation,Order book event study,The Price is a Lie screenshot
B04,Microstructure,Physical inventory squeeze analogy,SCREENSHOT,P2,BACKLOG,Inventory boundaries give nonlinear spread behavior,Fit to oil; only cautiously apply crypto,LQG oil seminar
B05,Microstructure,Futures price discovery,SCREENSHOT,P1,BACKLOG,Futures sometimes lead spot,Cross-venue time-lag and basis analysis,Financial vs Physical Barrels
P01,Portfolio,Mean variance CVaR allocation,MATH,P1,BACKLOG,Constrained diversified portfolio improves risk net,Compare fixed weight inverse vol constrained,portfolio notes
P02,Portfolio,Bellman inventory control,MATH,P2,BACKLOG,Sequential control outperforms greedy trades,MDP with honest simulator,control screenshot and user formula
P03,Portfolio,Option MM PnL decomposition,SCREENSHOT,P2,BACKLOG,Jump carry vol shocks separate risk,Independent PnL reconciliation,option MM screenshot
P04,Portfolio,Maker/taker fill model,HYPOTHESIS,P0,BACKLOG,Execution quality determines net alpha,Realistic queue based fill simulator,venue economics
P05,Portfolio,Graph central-peripheral allocation,HYPOTHESIS,P2,BACKLOG,Network position can improve risk budget,Compare simple beta/risk weighting,network portfolio screenshot
P06,Portfolio,Alpha potential games,SCREENSHOT,P3,BACKLOG,Approx potential game may simplify strategic interaction,Simple simulator with equilibrium baseline,University of Toronto talk
O01,Optimization,No Free Lunch optimizer comparison,MATH,P1,BACKLOG,No universal best on all objective functions,Fair benchmark runs,Peter Cotton screenshot
O02,Optimization,Wind Rose directional sensitivity,USER HYPOTHESIS,P2,BACKLOG,Objective anisotropy identifies stable parameter directions,Define exact method before run,user notes
O03,Optimization,Flood fill stable parameter basins,USER HYPOTHESIS,P2,BACKLOG,Broad parameter regions generalize better,Connected basin stability test,user notes
O04,Optimization,Smoothed simplex,PAPER,P2,BACKLOG,LP simplex has better smoothed than worst-case behavior,Toy LP and analysis of solver choices,arxiv.org/abs/2504.04197
I01,Infrastructure,Qlib modular components,REPO,P0,BACKLOG,Separable data model portfolio execution APIs,Engineering comparison/design audit,github.com/microsoft/qlib
I02,Infrastructure,TradingAgents research roles,REPO,P2,BACKLOG,Multi-agent research review might improve process,Source and data leakage audit,github.com/TauricResearch/TradingAgents
I03,Infrastructure,Kronos sequence model,REPO,P2,BACKLOG,Pretrained market model yields feature uplift,Controlled head-to-head vs GBDT TCN,github.com/shiyu-coder/Kronos
I04,Infrastructure,WorldMonitor macro/news,REPO,P2,BACKLOG,Global event dashboard aids context,Date fidelity and feed/source audit,github.com/koala73/worldmonitor
I05,Infrastructure,BlackJAX Bayesian sampling,REPO,P2,BACKLOG,Posterior samples improve risk calibration,Compare simpler inference methods,github.com/blackjax-devs/sampling-book
I06,Infrastructure,Causal backtest and evaluation report,IMPLEMENT,P0,BACKLOG,One shared cash ledger and reports for every run,Synthetic conservation plus holdout,QF-lib and Qlib screenshots
I07,Infrastructure,LLM research master prompt,USER NOTES,P1,SPECIFIED,Structured six-phase research improves traceability,Use evidence register and independent audit,user supplied master prompt
N01,Nonalpha,Macro instability 2025 LQG seminar,SCREENSHOT,P2,BACKLOG,Macro forces change risk premia and correlations,Rebuild official series with timestamps,LQG 2025 photos
N02,Nonalpha,Actuarial reserving models,SCREENSHOT,P3,BACKLOG,Reservers offer useful uncertainty modeling,Review actuarial context for transfer,textbook index image
N03,Nonalpha,Quant firms watchlist,USER NOTES,P2,SPECIFIED,Target institutions should be compared per role,Verify vacancies roles tech domains,user supplied list
N04,Nonalpha,Grant and fellowship leads,SCREENSHOT,P2,BACKLOG,Some programs may fund research work,Check official terms deadlines and location,grant list screenshot
N05,Nonalpha,LinkedIn professional summary,SCREENSHOT,P2,BACKLOG,Specific positioning helps communicate verified competence,Build quant-tailored evidence and narrative,profile screenshot
N06,Nonalpha,Physics alternative cosmology analogy,SCREENSHOT,P3,BACKLOG,Alternative gravity hypotheses need stand-alone tests,GR/ΛCDM comparison outside trading repo,physics meme screenshot
N07,Nonalpha,External Sharpe 2.5-3.0 model screenshot,SCREENSHOT,P3,BACKLOG,Attractive reported backtest not independently verified,Find original model/data or keep anecdotal,performance table image



## DATA IN

### IN

### OUT

---


## DATA CLEAN

### IN

### OUT

asset data

features
| feature_id | feature_name | description | how | 
|---|---|---| ---|
| 1 | Chronological_timestamp | this will be the baseline chronological clock | a |
| 2 | Event_timestamp | this will count the number of events | a |
| 3 | Open | The opening price of that timestamp | a |
| 4 | High | The highest price of that timestamp | a |
| 5 | Low | The lowest price of that timestamp | a |
| 6 | Close | The close price of that timestamp | a |
| 7 | Volume | The volume traded at that timestamp | a |
| 8 | Open_diff | (O(t) - O(t-1))/O(t-1) | a |
| 9 | High_diff | (H(t) - H(t-1))/H(t-1) | a |
| 10 | Low_diff | (L(t) - L(t-1))/L(t-1) | a |
| 11 | Close_diff | (C(t) - C(t-1))/C(t-1) | a |
| 12 | Volume_diff | (V(t) - V(t-1))/V(t-1) | a |
| 13 | Open_close_diff | close(t) - open(t) | a |
| 14 | Close_close_diff | C(t) - C(t-1) | a |
| 15 | ocd_ind | sign(close(t) - open(t)) | a |
| 16 | ccd_ind | sign(C(t) - C(t-1)) | a |
| 17 | High_low | H(t) - L(T) | a |
| 18 | Open_close | C(t)-O(t) | a |
| 19 | High_close | H(t) - C(t) | a |
| 20 | Close_low | C(t) - L(t) | a |
| 21 | HL_diff | (HL(t) - HL(t-1))/HL(t-1) | a |
| 22 | OC_diff | (OC(t) - OC(t-1))/OC(t-1) | a |
| 23 | HC_diff | (HC(t) - HC(t-1))/HC(t-1) | a |
| 24 | CL_diff | (CL(t) - CL(t-1))/CL(t-1) | a |
| 25 | Open_vol | The opening price volatility | a |
| 26 | High_vol | The highest price volatility | a |
| 27 | Low_vol | The lowest price volatility | a |
| 28 | Close_vol | The close price volatility | a |
| 29 | Volume_vol | The volume traded volatility | a |
| 30 | High_low_vol | H(t) - L(T) volatility| a |
| 31 | Open_close_vol | C(t)-O(t) volatility | a |
| 32 | High_close_vol | H(t) - C(t) volatility| a |
| 33 | Close_low_vol | C(t) - L(t) volatility | a |
| 34 | 3step_ind | ccd_ind on 3 steps | a |
| 35 | 5step_ind | ccd_ind on 5 steps  | a |
| 36 | 7step_ind| ccd_ind on 7 steps | a |
| 37 | 10step_ind | ccd_ind on 10 steps | a |

market data


---


## MARKET STATES

### IN


### transformations





### OUT

---


## REGIME DETECTION

### IN

### OUT

---


## ALPHA MATCH

### IN

### OUT

---


## ORDER MANAGEMENT

### IN

### OUT

---


## EXECUTION ENGINE

### IN

### OUT

---

