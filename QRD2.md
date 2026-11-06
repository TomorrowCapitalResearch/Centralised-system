# Quantitative Finance Research Notes — Part 2

These 10 additional screenshots expand the earlier collection in three particularly useful directions: time dynamics, conditional drawdown risk, and multiscale volatility decomposition.

Several images show different frames of the same model, so I'll consolidate them rather than repeat the same material.

## 13. Time Dynamics Model — Four dimensions of market behaviour

The first three screenshots provide the missing definitions for the previously discussed time-dynamics model:

\\[ Z(x,y)=F(\beta,\alpha,\tau,\nabla\tau;x,y,t) \\]

The model combines four quantities.

| Component                          | Formula shown                                                         | Interpretation                  |
| ---------------------------------- | --------------------------------------------------------------------- | ------------------------------- |
| Velocity / drift \\(\beta\\)       | \\(\mu/\sigma\\)                                                      | Risk-adjusted directional drift |
| Jerk \\(\alpha\\)                  | Mean second difference                                                | Changes in return dynamics      |
| Proper time deviation \\(\tau\\)   | \\(S_n/(\sigma\sqrt n)\\)                                             | Normalised cumulative deviation |
| Temporal gradient \\(\nabla\tau\\) | Standard deviation of return changes divided by mean absolute returns | Instability of return dynamics  |

### 13.1 Velocity — Risk-adjusted drift

The screenshot defines:

\\[ \boxed{\beta=\frac{\mu}{\sigma}} \\]

This is essentially a per-period signal-to-noise ratio for returns, rather than physical velocity.

A rolling implementation is:

\\[ \beta_t= \frac{\operatorname{mean}(r\_{t-W+1:t})} {\operatorname{std}(r\_{t-W+1:t})+\epsilon} \\]

A positive value indicates positive average returns relative to their dispersion; a negative value indicates the opposite.

Trading application: Compare risk-adjusted drift across assets and time horizons to help distinguish directional conditions from noisy markets.

### 13.2 Jerk — Second differences

The screenshot gives:

\\[ \boxed{ \alpha= \frac{1}{n-2} \sum\_{t=1}^{n-2} (r\_{t+2}-2r\_{t+1}+r_t) } \\]

This measures the average second difference of returns.

Strictly speaking, if returns are treated as the first differences of log prices, this is a third finite difference of log prices. That explains the use of the word jerk.

An important mathematical observation is that this sum telescopes:

\\[ \alpha= \frac{(r_n-r\_{n-1})-(r_2-r_1)} {n-2} \\]

Consequently, the mean largely depends on the differences at the endpoints, not the full internal path.

For regime classification, I would therefore also test:

\\[ J_t^{\mathrm{RMS}}= \sqrt{\frac1{W-2} \sum\_{i=t-W+3}^{t} (r_i-2r\_{i-1}+r\_{i-2})^2} \\]

This captures the magnitude of fluctuations in return acceleration without cancellation between positive and negative values.

### 13.3 Proper time deviation

The third screenshot defines:

\\[ \boxed{ \tau=\frac{S_n}{\sigma\sqrt n}, \qquad S_n=\sum\_{t=1}^{n}(r_t-\mu) } \\]

This resembles a normalised cumulative-sum statistic.

There is a crucial detail: if \\(\mu\\) is the sample mean calculated over exactly those \\(n\\) observations, then \\(S_n=0\\) identically.

A meaningful implementation therefore needs a separately specified reference drift, such as a historical estimate fixed before the evaluation window.

For example:

\\[ \tau_t= \frac{\sum\_{i=t-W+1}^{t}(r_i-\mu\_{\mathrm{ref}})} {\sigma\_{\mathrm{ref}}\sqrt W} \\]

This can indicate how far the current return process has deviated from an established reference regime.

### 13.4 Temporal gradient

From the earlier screenshot:

\\[ \nabla\tau= \frac{\operatorname{sd}(|\Delta r_t|)} {\operatorname{mean}(|r_t|)} \\]

where the original uses \\(r\_{t+1}-r_t\\). In live trading, this must be calculated with historical differences only.

A causal rolling version could be:

\\[ G_t= \frac{\operatorname{std}\_W(|r_i-r\_{i-1}|)} {\operatorname{mean}\_W(|r_i|)+\epsilon} \\]

Together, these four features could form a useful time-dynamics vector:

\\[ \mathbf T_t= (\beta_t,\alpha_t,\tau_t,G_t) \\]

The proposed research question is whether this vector improves future volatility, direction or regime-transition forecasts beyond simpler return and volatility features.

## 14. Enhanced Conditional Drawdown-at-Risk — KERR and SCHW

Screenshots 5 and 6

This is the most immediately useful addition for portfolio management.

The screenshots describe two enhanced CDaR models, labelled KERR and SCHW, that adjust position size using:

\\[ \boxed{ \text{Position} =f( \text{CDaR depth}, \text{recovery strength}, \text{vol-of-vol}, \text{regime} ) } \\]

### 14.1 Standard Conditional Drawdown-at-Risk

Let portfolio equity be \\(E_t\\).

Define the running peak:

\\[ M_t=\max\_{s\le t}E_s \\]

and fractional drawdown:

\\[ D_t=1-\frac{E_t}{M_t} \\]

For confidence level \\(\alpha\\), define the drawdown quantile:

\\[ d\_\alpha=\operatorname{VaR}\_\alpha(D) \\]

Then:

\\[ \boxed{ \operatorname{CDaR}\_\alpha = \mathbb E[D\mid D\ge d\_\alpha] } \\]

This is the conditional expectation of drawdown in the worst tail of the drawdown distribution, subject to the usual care about quantile ties.

Unlike ordinary VaR, CDaR directly describes portfolio losses relative to previous equity peaks.

### 14.2 Enhanced CDaR

The post proposes extending the basic risk measure using three additional dimensions.

Drawdown depth

How far has the portfolio fallen from its running maximum?

Recovery strength

How quickly is the portfolio recovering after a drawdown?

Possible measurements include recovery slope, time since trough and the fraction of the previous drawdown recovered.

Volatility of volatility

How unstable is the volatility process itself?

For example:

\\[ VoV_t=\operatorname{std}\_W(\hat\sigma_i) \\]

A volatility estimate rising sharply and erratically may indicate a less stable risk environment.

### 14.3 Dynamic allocation

A candidate model is:

\\[ w_t=w_t^{\mathrm{base}} \cdot f_D(D_t,\operatorname{CDaR}\_t) \cdot f_R(\text{recovery}\_t) \cdot f_V(VoV_t) \cdot f_Z(Z_t) \\]

Each adjustment should be estimated and validated rather than chosen solely by intuition.

For example, a risk multiplier might decrease when conditional drawdown risk rises, but this could also cause the strategy to sell after losses and miss recoveries.

### 14.4 Results displayed in the screenshots

One example dated 15 July 2011 shows the following approximate portfolio values:

| Strategy     | Portfolio value | Return from $100 |
| ------------ | --------------- | ---------------- |
| Buy and hold | $103            | +3%              |
| SCHW         | $132            | +32%             |
| KERR         | $167            | +67%             |

These are values displayed in the post, not independently verified backtest results. They are also one snapshot rather than a complete performance evaluation.

The caption says the enhanced CDaR strategies were tested using walk-forward evaluation, which is the right general direction, but the screenshots do not provide enough information to verify the procedure, costs, parameter selection or robustness.

Research priority: Implement standard CDaR first, then test whether recovery strength and volatility-of-volatility improve out-of-sample drawdown control.

## 15. SHIELD — Field-based portfolio risk control

Screenshot 7

Another screenshot presents a PhD thesis proposal involving a field-based risk model.

The visible expression has the general structure:

\\[ s_t= \frac{\nu}{\sqrt T\hat\sigma_t} \cdot \Phi(\Lambda_t^{(\alpha)}) \cdot \mathcal K[\mathfrak M_t] \\]

The model also includes a spatial integral of a field against a weighting function.

Its exact definitions are not supplied, so the model cannot be reproduced from the screenshot alone.

The central idea appears to be combining volatility scaling with a nonlinear transformation of market-state information and a field-derived adjustment.

A screenshot dated 7 December 2007 displays approximately:

| Strategy     | Portfolio value |
| ------------ | --------------- |
| Buy and hold | $118            |
| GAUNTLET     | $116            |
| SHIELD       | $119            |

The small displayed difference is not enough to establish that SHIELD improves performance.

### Practical interpretation

The useful concept is a risk-control layer that responds to a multidimensional representation of the market, rather than just realised volatility.

For a trading system, this could mean:

\\[ \text{Exposure}\_t = f( \text{forecast volatility}, \text{market turbulence}, \text{drawdown risk}, \text{liquidity}, \text{signal confidence} ) \\]

The model's contribution would need to be tested against ordinary volatility targeting and simpler drawdown controls.

## 16. CVRC — Additional details on multiscale volatility

Screenshot 10

The new caption clarifies the earlier CVRC model.

It says that convolution operations are used to decompose return variances across trading frequencies, with the aim of identifying whether market activity is concentrated in:

- Noise.
- Cycles.
- Persistent trends.

This gives us a more specific research direction than merely plotting realised volatility at different horizons.

### Multiresolution decomposition

A candidate implementation could use wavelets or a bank of convolution filters to decompose returns:

\\[ r_t=\sum\_{j=1}^{J}d\_{j,t}+a\_{J,t} \\]

where \\(d\_{j,t}\\) represents activity at different scales and \\(a\_{J,t}\\) is the residual low-frequency component.

We can then calculate energy at each scale:

\\[ E\_{j,t}=\sum\_{i=t-W+1}^{t}d\_{j,i}^2 \\]

and normalised energy shares:

\\[ q\_{j,t}=\frac{E\_{j,t}}{\sum_k E\_{k,t}+\epsilon} \\]

These features may reveal whether recent movement is dominated by short-scale fluctuations or longer-scale behaviour.

A scale-entropy statistic is:

\\[ H_t^{\mathrm{scale}}=-\sum_jq\_{j,t}\log q\_{j,t} \\]

This would complement the state-occupation entropy extracted from ZEROLINE.

A qualification: frequency decomposition alone does not reliably separate noise, cycles and trends. That interpretation requires additional statistical tests. Likewise, the caption's suggestion that variance decomposition locates volume concentration is not established without volume data.

## 17. Updated integrated feature inventory

Combining both batches of screenshots gives us the following candidate research features.

| Category             | Features                                                               |
| -------------------- | ---------------------------------------------------------------------- |
| Direction            | Drift, risk-adjusted drift, directional efficiency                     |
| Return dynamics      | Second differences, normalised cumulative deviation, temporal gradient |
| Volatility           | Realised volatility, multiscale volatility, volatility of volatility   |
| Frequency structure  | Scale energy, scale entropy, convolutional features                    |
| Jump dynamics        | Jump intensity, discrete level crossings, transition persistence       |
| Regime structure     | State occupation, state entropy, transition probabilities              |
| Drawdown             | Current depth, CDaR, recovery slope, recovery duration                 |
| Market stability     | Roughness, turbulence, dispersion, cross-asset dependence              |
| Portfolio allocation | Volatility scaling, regime multiplier, drawdown multiplier             |
| Nonlinear modelling  | Composite fields, nonlinear state embeddings                           |

## 18. How I would incorporate these into your research

The strongest approach is to treat each model as a set of testable feature hypotheses rather than as a ready-made trading strategy.

For your minute-level cryptocurrency research, I would use three successive stages.

Stage A — Feature validation. Calculate the proposed features causally across BTC, ETH and other liquid assets. Test their persistence, stability, cross-asset relationships and incremental predictive value.

Stage B — Market-state prediction. Compare simple benchmarks against models that predict future volatility, jump probability, direction and transition likelihood over horizons such as 1, 5, 15, 30 and 60 minutes.

Stage C — Trading and portfolio validation. Feed only validated forecasts into your existing trend, reversion and breakout strategies. Compare fixed sizing, volatility targeting, CDaR controls and regime-adjusted allocation, with realistic Revolut X trading costs and execution assumptions.

The two most promising additions from this batch are Enhanced CDaR for portfolio risk management and multiscale variance decomposition for market-state detection. The time-dynamics features are also worth testing, but some of the formulas need careful correction or interpretation before they can be used.

The overarching research objective is now clearer: build a system that estimates not only where prices may move, but also how the market is moving, how stable that behaviour is, and how much risk the portfolio should take under those conditions.



I've received the additional screenshots from Quant Research Decoded. They expand the collection into several useful areas of quantitative finance, particularly market microstructure, volatility modelling, regime classification, tail-risk estimation, and probability-based position sizing.

The most important concepts in this batch are:

| Research area            | Model or concept                                        | Potential application                                    |
| ------------------------ | ------------------------------------------------------- | -------------------------------------------------------- |
| Market microstructure    | Drift–volatility surfaces and evolving return densities | Detect distributional regime shifts                      |
| Regime classification    | ZEROLINE σ.1, jump clustering and occupation density    | Identify persistent market states                        |
| Cross-venue liquidity    | VenueFrag and VenueVolSpread                            | Detect liquidity fragmentation and execution risk        |
| Volatility modelling     | Λ-Vol and feedback dynamics                             | Model volatility propagation and instability             |
| Tail-risk modelling      | VaR reliability surfaces                                | Detect when risk-model assumptions break down            |
| Market-state geometry    | Momentum–volatility regime surfaces                     | Select strategies conditional on market conditions       |
| Order-flow dynamics      | Churn, liquidity bursts and memory surfaces             | Distinguish persistent activity from temporary shocks    |
| Cross-asset structure    | Sector and factor coupling                              | Detect correlation breakdowns and market rotations       |
| Probability-based sizing | Distribution concentration, entropy and multimodality   | Adjust exposure based on estimated outcome distributions |

A particularly useful distinction is between descriptive models, which characterize current market conditions, predictive models, which estimate future outcomes, and decision models, which convert those estimates into trading positions.

The screenshots contain interesting research hypotheses, but the visualizations alone do not establish predictive power or profitability. For example, a concentrated probability distribution does not automatically imply a positive expected return, and a regime surface needs independently validated definitions before its geometric features can guide trading decisions.

These concepts fit naturally into the broader research framework we've been developing:

Market data → Feature extraction → Market-state estimation → Predictive distributions → Strategy selection → Position sizing → Risk management → Execution → Performance validation.

I can continue consolidating the screenshots into structured research notes, preserving the mathematical definitions, proposed mechanisms, practical applications, and hypotheses that would need empirical testing.
