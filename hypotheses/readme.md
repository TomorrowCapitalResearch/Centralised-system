notes:
github.com/koala73/worldmonitor
github.com/shiyu-coder/Kronos - a foundation model for the language of financial markets
github.com/TauricResearch/TradingAgents

V(s)=max_a(R(s,a)+ gamma * V(s'))
your expected value now = take the action that maximizes return(immediate return from action + your expected future value)


genetic algorithms, ml, nn

search for edge to exploit
objective mono and multi-parameter optimization(Wind Rose, Flood fill)

validation of the strategy using conventional and non-conventional analyses derived from periodic signal theory
From satelite to core: integrating quantitative strategies into total portfolio allocation - Aous labbane, head of business development, Investcorp



how to proceed: quants are first and foremost pessimistic and lean of the objective guide that is the maths. The maths is riddled with assumptions - identify them.
be curious always, and protect yourself against becomign dogmatic, stare into the abyss and the abyss stares back. pessimism is a guide not a personality trait. go too deep and youll become too distrusting.

too dstrusting is itself a strategy it drives one to close themselves off completely.

optimise portfolio allocation like citadel portfolio managers
engineer quantitative trading strategies like citadel senior analysts
stress test strategies against historical data like two sigma researchers
architect risk management systems like bridgewater associates managers
hunt for exploitable patterns like renaissance technologies quants
run technical analysis like goldman sachs vp level strategists
run fundamental analysis like
break down facktor investing like AQR Capital management researchers
macro indicators like bridgewater associates 
build concentrated position thesis like pershing square analysts
design option strategies like DE SHAW options strategists.

treat the spread as a time series made of structured components plus transient noise. SSA embeds the spread into a trajectory matrix, decompose it into singular...

Trajectory matrix, X = [s1, s2, ..., sK, // s2, s3, ... sK+1 // s3, s4, ... // // // sL, sL+1, ... sT], K=T-L+1
Singular decomposition X = SUM(1, r) sigma_j, u_j v_j^T
signal-like reconstruction X_sig = SUM(j in S) sigma_j, v_j v_j^T
denoised spread s~t = DiagonalAverage(X_sig)
noise component n_t = s_t - s~t
entry signal z_t = (s~t - Mean(s~_(t-W:t-1)))/std(s~_(t-W:t-1))
trade gate abs(z_t) > tau and abs(n_t) < eta implying spread move is persistent enough to trade
exit rule abs(z_t) <= epsilon or abs(n_t) up implying close/reduce to avoid noise-driven exposure


diffusion limit(large rates)


rentec, tgs, xtx markets are at the top

hrt, radix, citadel, headlands, quadrature, pdt partners, de shaw, jump, citadel, jane street,

two sigma, imc, voleon group, g research, xanthum, drw, tower, millennium, point72, optiver, five rings, SIG

qube rt, virtu, squarepoint, ansatz, aquatic, vatic labs, arrowstreet, marshall w. engr. gate, old mission, gsa capital, schonfeld,

aqr, akuna, maven securities, ctc, voloridge, quantlab, man group, cfm, exoduspoint, verition, balyasny, worldquant, flow traders

belvedere, transmarket, gelber group, valkyrie, wolverine, peak6, scientech, group one, xr trading, dv trading, teza, trexquant


there truly is no free lunch in optimization.
Optimiser
|Optimiser|Rosenbrock(smooth valley)| RASTRIGIN(multimodal)|ACKLEY(FLAT+BUMPY)|LENIS(RUGGED NEEDLE)| TETRIS(NOISY) PLINKO(NOISY) CREATURE(EMERGENT) GENETICART(HIGH DIM) ANTENNA(MULTIMODAL) EBOLA(CONTROL) ROBOTARM(CONSTRAINED) BRACHISTO(SMOOTH) ROCKET(CONTROL) BATTERY(ARBITRAGE)
|---|---|---|---|---|
|PRIMA_BOBYOA|   |   |   |   |
|Differential Evolution|   |   |   |   |
|Particle Swarm|   |   |   |   |
|CMA-ES|   |   |   |   |
|Harmony Search|   |   |   |   |
|Random Search|   |   |   |   |
|Genetic Algorithm|   |   |   |   |
|Nelder-Mead|   |   |   |   |
|Powell|   |   |   |   |
|Simulated Annealling|   |   |   |   |
|Evolution Strategy|   |   |   |   |

## mean reversion


## drift follow

## gap filling

## volatility/turbulence

## Breakout



## Master prompt
the output quality comes from the order of operations, not from one clever prompt. Six phases, each one feeding the next.

1. plan firs, search second. split the topic into four to eight strands that do not overlap and together cover the whole question. Good decomposition is the single largest leve on the final quality.
2. research in parallel, with citation discipline. One worker per strand, each recording the same structured fields for every source, and keeping anything it could not open in a separate unverified list.
3. force the shape of a field. Turn raw findings into consensus, disagreements, common ground, limitations, gaps and breakthroughs. This is the step that makes a review useful rather than a pile of summaries.
4. turn the synthesis into data.
5. verify independently.
6. deliver two forms.

'ROLE
you are a senior quant research lead. produce a rigorous, citation accurate review of the topic below,
and deliver two things: an interaction dashboard and a plain language article with a quantitative lens. Optimise for correctness and decision usefulness, not length.

BRIEF
TOPIC: {{the research question, as specific as you can make it}}
AUDIENCE: {{who reads this and what they decide, e.g. a quant PM with a maths background}}
TIME WINDOW: {{e.g. the last five years, plus the foundational work before it}}
QUANT LENS: {{the mathematical angle to foreground, e.g. stochastic models, option pricing, econometrics, optimisation}}
DEPTH: {{fast scan, standard, exhaustive}}

PHASE 1, PLAN
break the topic into four to eight strands that do not overlap and together cover the whole question.
