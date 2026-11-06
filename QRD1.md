# Quantitative Finance Research Notes — Extracted from 10 Screenshots

The screenshots come from Quant Research Decoded and cover several connected ideas: volatility forecasting, regime classification, jump clustering, mathematical physics applied to financial markets, and dynamic position sizing.

The most useful underlying concept is that market behaviour should be classified across multiple dimensions, and trading strategies and position sizes should adapt to those dimensions.

I'll separate what is actually presented in the screenshots from the mathematical interpretation, potential trading applications, and claims that still require validation.

## 1. Overview of the extracted models

| Model / concept              | Main purpose                                                       | Potential trading application                |
| ---------------------------- | ------------------------------------------------------------------ | -------------------------------------------- |
| ZERO­LINE_δ.1                | Regime classification using jump clustering and discrete states    | Detect transitions between market regimes    |
| HARMONIC_Ψ.1 / CVRC          | Volatility resolution clustering across time scales                | Forecast volatility expansion                |
| TITAN_0.1                    | Implied-volatility surface and regime probabilities                | Volatility forecasting and risk allocation   |
| WAVE_σ.9                     | Distinguish smooth trends from choppy price movement               | Trend versus mean-reversion selection        |
| ζ-Field                      | Combine multiple mathematical fields into a market-state surface   | Multidimensional regime representation       |
| Time Dynamics Model          | Measure changing temporal characteristics of returns               | Identify unstable or accelerating conditions |
| Effective Mass / Temperature | Translate spatial dispersion into a risk indicator                 | Adaptive leverage and position sizing        |
| Position Size Optimisation   | Change exposure without necessarily changing directional forecasts | Portfolio risk management                    |

These are names and descriptions from the posts, not independently established or validated quantitative models.

## 2. ZERO­LINE_δ.1 — Jump clustering and regime classification

Screenshot 1

This model discretises a return-related process into levels and measures how frequently the process moves between those levels.

### Mathematical construction

The screenshot gives:

\\[ n(t)=\left\lfloor \frac{R_t-R\_{\min}}{\Delta R}(N_L-1) \right\rfloor \\]

where:

- \\(R_t\\): cumulative return or another return-derived state variable.
- \\(R\_{\min}\\): lower boundary.
- \\(\Delta R\\): scaling range.
- \\(N_L\\): number of discrete levels.
- \\(n(t)\\): current discrete state.

For a bounded state variable, this should be clipped to \\([0,N_L-1]\\), and the scaling parameters should be estimated using historical information only.

The screenshot then defines upward and downward level-crossing events:

\\[ \Delta n_t=n_t-n\_{t-1} \\]

\\[ a_t^\dagger= \mathbf 1\_{\\{\Delta n_t>0\\}}|\Delta n_t| \\]

\\[ a_t= \mathbf 1\_{\\{\Delta n_t<0\\}}|\Delta n_t| \\]

These are labelled creation and annihilation events, borrowing terminology from quantum mechanics.

Their practical meaning is much simpler: upward and downward movements across discrete return levels.

The rolling occupation density is:

\\[ P(n,t)=\frac{1}{W} \sum\_{\tau=t-W+1}^{t}\delta\_{n,n\_\tau} \\]

This measures the fraction of observations spent in each discrete level over a rolling window.

### Useful features to extract

We can derive:

State occupancy entropy

\\[ H_t=-\sum_n P(n,t)\log P(n,t) \\]

Transition intensity

\\[ J_t=\frac{1}{W}\sum\_{\tau=t-W+1}^{t}|\Delta n\_\tau| \\]

Directional imbalance

\\[ I_t=\frac{\sum a\_\tau^\dagger-\sum a\_\tau} {\sum a\_\tau^\dagger+\sum a\_\tau+\epsilon} \\]

State persistence

\\[ S_t=\frac{1}{W-1}\sum\_{\tau} \mathbf1\_{\\{n\_\tau=n\_{\tau-1}\\}} \\]

These quantities could help distinguish stable, directional, turbulent and transitional regimes.

The screenshot also reports a sample entropy of 36%, purity of 38%, and an occupation-related statistic. Their exact normalisations are not sufficiently documented to reproduce them.

### Trading hypothesis

When state occupancy becomes more dispersed and transitions become more frequent, the market may be entering a turbulent regime.

Test whether increases in \\(H_t\\) and \\(J_t\\) predict future realised volatility, jumps or trend breakdowns.

Important: This does not automatically imply a sell signal. Increasing volatility can accompany both upward and downward moves.

## 3. HARMONIC_Ψ.1 — Convolutional Volatility Resolution Clustering

Screenshot 2

This is one of the more interesting ideas in the collection.

The model represents volatility across two dimensions:

- Time, or observation window.
- Resolution, measured in weeks in the screenshot.

The resulting 3D surface contains peaks and ridges corresponding to volatility concentrations at different scales.

### Mathematical interpretation

Define realised volatility over horizon \\(h\\):

\\[ \sigma_t(h)= \sqrt{\frac{A}{h} \sum\_{i=0}^{h-1}(r\_{t-i}-\bar r\_{t,h})^2} \\]

where \\(A\\) is an annualisation factor appropriate to the sampling frequency.

We can construct a multiscale volatility surface:

\\[ V(t,h)=\sigma_t(h) \\]

and apply convolutional smoothing:

\\[ \widetilde V(t,h) =\sum\_{i,j}K(i,j)V(t-i,h_j) \\]

The exact CVRC kernel and algorithm are not disclosed in the screenshots, so this is a proposed reconstruction rather than the author's implementation.

### Trading application

Potential features include:

- Volatility-surface slope across horizons.
- Surface curvature.
- Concentration of high-volatility regions.
- Volatility clustering persistence.
- Short-horizon versus long-horizon volatility ratios.
- Speed of volatility-surface expansion.

For example:

\\[ Q_t=\frac{\sigma_t(5)}{\sigma_t(60)} \\]

where the horizons refer to five and sixty bars.

If short-horizon volatility rises sharply relative to long-horizon volatility, this may signal a developing volatility shock.

The valuable research question is whether the multiscale surface predicts future volatility better than standard HAR-RV, EWMA or GARCH benchmarks.

## 4. TITAN_0.1 — Implied volatility and regime probabilities

Screenshot 7

The displayed model uses an implied-volatility surface with:

- Moneyness \\(K/S\\).
- Time to expiration.
- Implied volatility.
- Regime probabilities.

The screenshot shows:

\\[ P(\text{Low})=59\\%,\quad P(\text{Medium})=40\\%,\quad P(\text{High})=1\\% \\]

These are an example of the model's output, not probabilities we can independently verify.

### Potential feature engineering

Given an implied-volatility surface:

\\[ \sigma\_{\mathrm{IV}}(K/S,\tau,t) \\]

we can calculate:

| Feature                      | Interpretation                                     |
| ---------------------------- | -------------------------------------------------- |
| ATM implied volatility       | Market-implied volatility level                    |
| Term-structure slope         | Near-term versus longer-term uncertainty           |
| Skew                         | Relative pricing of downside versus upside options |
| Surface curvature            | Smile shape and tail-risk pricing                  |
| IV minus realised volatility | Implied versus historical volatility premium       |
| Surface change               | How option-market expectations are evolving        |

A regime model could estimate:

\\[ P(Z\_{t+h}=k\mid\mathcal F_t) \\]

for \\(k\in\\{\text{low, medium, high volatility}\\}\\).

For crypto trading, this may be particularly interesting for BTC and ETH, where options-market information can complement spot and futures data.

However, an implied-volatility surface cannot be reconstructed from spot OHLCV alone. Historical options quotes would be required.

## 5. WAVE_σ.9 — Smooth trends versus turbulent markets

Screenshot 8

The post makes an important distinction:

> A smooth upward trend and a choppy upward trend can have similar net returns but very different trading characteristics.

It proposes using the geometry of a surface to distinguish smooth directional movement from repeated oscillations.

### A practical reconstruction

Define directional efficiency:

\\[ ER_t= \frac{|p_t-p\_{t-W}|} {\sum\_{i=t-W+1}^{t}|p_i-p\_{i-1}|} \\]

where \\(p_t\\) is log price.

This quantity lies between zero and one.

- High \\(ER_t\\): movement is relatively directional.
- Low \\(ER_t\\): movement is relatively oscillatory.

We can also calculate realised variance:

\\[ RV_t=\sum\_{i=t-W+1}^{t}r_i^2 \\]

and a roughness measure:

\\[ C_t= \frac{\sum\_{i=t-W+2}^{t}|r_i-r\_{i-1}|} {\sum\_{i=t-W+1}^{t}|r_i|+\epsilon} \\]

Together these describe different aspects of market movement.

### Trading implications

| Market condition                 | Candidate approach                                     |
| -------------------------------- | ------------------------------------------------------ |
| High efficiency, low roughness   | Trend following                                        |
| Low efficiency, high roughness   | Mean reversion, subject to costs                       |
| High efficiency, high volatility | Momentum with reduced risk                             |
| Low efficiency, high volatility  | Defensive positioning or selective liquidity provision |
| Low volatility, rising roughness | Watch for a possible regime transition                 |

The screenshot's claim that a 3D surface reveals future strategy P&L is not established by the image. It would require prospective validation.

## 6. ζ-Field — Combining multiple mathematical fields

Screenshot 9

The post shows a proprietary composite field:

\\[ \Xi_t(x,y)= \operatorname{Norm}\_{[0,100]} \left[ V+F+C+B+S+A+H+J+Q+T \right] \\]

The terms have additional arguments in the original expression, but their precise definitions are not supplied.

The broad concept is a multidimensional representation of market state, incorporating several interacting effects.

### Useful interpretation

Instead of representing a market with a single regime label, define a state vector:

\\[ \mathbf z_t= \begin{bmatrix} \text{trend}\\\ \text{mean reversion}\\\ \text{volatility}\\\ \text{jump intensity}\\\ \text{liquidity}\\\ \text{market breadth}\\\ \text{turbulence}\\\ \text{cross-asset dependence} \end{bmatrix} \\]

Then a transformation:

\\[ \Xi_t=f\_\theta(\mathbf z_t) \\]

could produce a low-dimensional state representation.

This could be constructed using PCA, autoencoders, Gaussian mixtures, hidden Markov models or a carefully specified composite score.

Research warning: Combining many indicators into one visually impressive surface does not guarantee more predictive information. Feature redundancy, unstable scaling and overfitting become major concerns.

## 7. Time Dynamics Model — Temporal gradients

Screenshot 10

The post displays:

\\[ Z(x,y)=F(\beta,\alpha,\tau,\nabla\tau;x,y,t) \\]

and defines a temporal gradient:

\\[ \nabla\tau= \frac{\operatorname{sd}(|\Delta r_t|)} {\operatorname{mean}(|r_t|)} \\]

with:

\\[ \Delta r_t=r\_{t+1}-r_t \\]

This is a measure of variability in changes of returns, relative to the typical absolute return.

### Important look-ahead issue

The screenshot explicitly uses \\(r\_{t+1}\\).

That is future information when predicting at time \\(t\\).

For a causal implementation, define:

\\[ \Delta r_t=r_t-r\_{t-1} \\]

and compute the rolling feature using only observations available at the decision time.

A candidate implementation is:

\\[ G_t= \frac{ \operatorname{std}\_{W}(r_i-r\_{i-1}) }{ \operatorname{mean}\_{W}(|r_i|)+\epsilon } \\]

This may help detect changes in market roughness, although its predictive value remains untested.

The accompanying S&P 500 chart shows a strategy line outperforming buy-and-hold over the displayed period, but does not establish out-of-sample performance.

## 8. Effective Mass and Temperature — Physics-inspired risk allocation

Screenshot 4

The post references the Hawking temperature equation:

\\[ T_H=\frac{\hbar c^3}{8\pi GMk_B} \propto\frac{1}{M} \\]

It then proposes an effective mass based on pairwise distances between points:

\\[ M\_{\mathrm{eff}}= \frac{2}{n(n-1)} \sum\_{i\<j}\\|\mathbf r_i-\mathbf r_j\\| \\]

The screenshot writes this in terms of pairwise spatial distances, followed by an inverse relationship:

\\[ T=\frac{1}{M\_{\mathrm{eff}}} \\]

The proposed trading interpretation is:

- High temperature: reduce exposure.
- Low temperature: more stable conditions, potentially permitting greater exposure.

### Mathematical caveat

The physics analogy is not a derivation of financial risk.

In particular, larger pairwise dispersion produces larger \\(M\_{\mathrm{eff}}\\) and therefore lower \\(T\\). Depending on how the points are defined, greater dispersion could actually represent greater market instability.

Therefore, the mapping between mass, temperature and risk must be empirically established.

### Better trading implementation

Treat the proposed temperature as a candidate feature:

\\[ T_t=\frac{1}{M\_{\mathrm{eff},t}+\epsilon} \\]

Then test whether it predicts:

- Future realised volatility.
- Maximum adverse excursion.
- Future drawdown.
- Cross-asset correlation changes.
- Liquidity deterioration.

Only after validation should it influence position sizing.

## 9. Dynamic position sizing

Screenshots 3, 5 and 10

This is perhaps the most immediately applicable idea.

One screenshot displays a position-sizing expression of the approximate form:

\\[ g_t= \frac{\nu}{\sqrt{T}\hat\sigma_t} \cdot \Psi(\Gamma_t^{(\beta)}) \cdot \mathcal J[\tilde s_t] \\]

The exact definitions of \\(\Psi\\), \\(\Gamma\\) and \\(\mathcal J\\) are not given.

Its broad structure is:

\\[ \text{Position size} = \text{Volatility scaling} \times \text{Market-state adjustment} \times \text{Signal adjustment} \\]

### Practical formulation

For asset \\(i\\):

\\[ w\_{i,t}= \operatorname{clip} \left( \frac{\sigma\_{\mathrm{target}}} {\hat\sigma\_{i,t}+\epsilon} \\,c\_{i,t}\\,s\_{i,t}, -w\_{\max},w\_{\max} \right) \\]

where:

- \\(\sigma\_{\mathrm{target}}\\): target risk.
- \\(\hat\sigma\_{i,t}\\): forecast volatility.
- \\(c\_{i,t}\\): regime-dependent confidence or risk multiplier.
- \\(s\_{i,t}\\): signed trading signal.
- \\(w\_{\max}\\): maximum exposure.

This formulation can be applied to trend following, mean reversion or breakout strategies.

### Critical point from the caption

The author explicitly warns that full-sample means and standard deviations, as well as future returns \\(r\_{t+1}\\), can introduce look-ahead bias.

This is one of the most valuable observations in the entire collection.

For a legitimate backtest:

- All features must be available before the order decision.
- Rolling normalisation must use historical observations.
- Parameters must be fitted only on training data.
- Orders must be executed at achievable prices.
- Transaction fees, spread, slippage and partial fills must be included.

## 10. An integrated research framework

The screenshots collectively suggest a system with three main layers.

Layer 1 — Market observations

OHLCV · Order book · Trades · Futures · Options · Cross-asset data

Volatility

Multiscale RV, IV, clustering

Market structure

Trend, efficiency, roughness

Transitions

Jumps, entropy, occupancy

Dependence

Breadth, correlations, lead–lag

Layer 2 — Regime engine

Probabilities of direction, volatility, turbulence and transition

Strategy selection

Trend, reversion, breakout, defensive

Risk allocation

Sizing, inventory, leverage, limits

Layer 3 — Execution and validation

Net P&L · Drawdown · Turnover · Fill probability · Calibration · Out-of-sample results

### Research priorities for your crypto trading system

Given your interest in minute-level crypto strategies and adaptive market states, I would prioritise the following experiments:

| Priority | Experiment                                | Primary test                                                      |
| -------- | ----------------------------------------- | ----------------------------------------------------------------- |
| 1        | WAVE-style trend efficiency and roughness | Does it improve trend/reversion strategy selection?               |
| 2        | Multiscale volatility surface             | Does it predict volatility better than HAR-RV/EWMA?               |
| 3        | ZEROLINE jump clustering                  | Does state-transition intensity predict future jumps?             |
| 4        | Regime-adjusted position sizing           | Does it improve risk-adjusted net returns?                        |
| 5        | Cross-asset volatility and breadth        | Does market-wide information improve individual-asset forecasts?  |
| 6        | Composite ζ-Field                         | Does nonlinear feature combination outperform simpler models?     |
| 7        | Effective-mass temperature                | Does it add predictive value beyond ordinary dispersion measures? |
| 8        | Options-implied volatility surface        | Does IV add information beyond spot/futures features?             |

These should be tested as separate hypotheses before combining them.

## 11. What is credible, and what remains speculative?

| Claim or idea                                                 | Assessment                                    |
| ------------------------------------------------------------- | --------------------------------------------- |
| Volatility clustering                                         | Well-established empirical phenomenon         |
| Multiscale volatility modelling                               | Established and worth testing                 |
| Regime-dependent position sizing                              | Sound research direction                      |
| Return-path roughness matters                                 | Reasonable and testable                       |
| Jump clustering may reveal transitions                        | Plausible, requires forecasting tests         |
| 3D surfaces predict future strategy P&L                       | Not demonstrated                              |
| Volatility spikes can reliably be predicted before they occur | Not demonstrated                              |
| Physics-inspired effective temperature measures market risk   | Speculative                                   |
| Proprietary ζ-Field offers superior forecasts                 | Cannot assess without definitions and results |
| Attractive backtest charts prove profitable strategies        | No; insufficient evidence                     |

A particularly important distinction is between descriptive state estimation and predictive state forecasting. A model may classify today's market accurately without providing any useful information about tomorrow's market.

Similarly, a volatility forecast is not automatically a profitable trading signal. Its value depends on how it changes trade selection, execution and risk.

## 12. Recommended research outputs

For each model, I would preserve a consistent research record containing its mathematical definition, feature inputs, forecast target, causal implementation, baseline comparison, test results and limitations.

The first four models I would implement are:

1. Market roughness and directional efficiency, to distinguish trend from oscillation.
2. Multiscale volatility clustering, to estimate volatility states and transitions.
3. Discrete-state jump and occupation statistics, to measure jump intensity, persistence and entropy.
4. Dynamic risk and inventory sizing, to translate validated state information into portfolio decisions.

For Revolut X, these should ultimately be evaluated against maker/taker fees, realistic fill assumptions and the actual net returns of your strategies.

The main insight worth retaining from these screenshots is not the physics terminology or the 3D visualisations. It is the possibility of constructing a multidimensional, probabilistic market-state model that controls both strategy selection and position sizing, with every component independently validated before integration.
