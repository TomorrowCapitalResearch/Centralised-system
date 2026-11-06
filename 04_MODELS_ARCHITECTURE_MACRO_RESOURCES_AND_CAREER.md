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
