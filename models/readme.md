
# fundamental idea

The fundamental idea in trading:
 **fair value depends on who else is in the market.** The asset is worth whatever the next counterparty will pay, and that depends on whether anyone shows up, how many show up, and how well informed they are.

## The core statement

> **Trading is the act of standing between counterparties whose valuations disagree, and capturing part of the gap. How big a gap exists, and how much of it you keep, depends on how much interest there is and how much competition you face.**

Or more compactly: *buy from those who value it less than the next buyer will; sell to those who value it more than the last seller did.*

## The two axes

Your three scenarios fit on two dimensions:

| | **Low competition** | **High competition** |
|---|---|---|
| **High interest** | *Price setter.* You're the only one supplying liquidity, so you set the spread and earn a rent. Your own impact is the dominant force. | *Error harvester.* The price is roughly efficient, and you profit only from mismatches between buyers and sellers. Your edge shrinks as more skilled players join. |
| **Low interest** | *Illiquid.* There's nobody to trade with. Price is undefined or meaningless, and any trade happens at a large discount or premium. | *Crowded and dead.* Many traders chase a few counterparties. Spreads collapse and nobody makes money. |

**Interest** sets how much flow exists. **Competition** sets who captures it.

## The errors you harvest in the competitive quadrant

These are the drifts and reversions you described. They come from three sources:

1. **Information errors.** A counterparty doesn't know something yet: news, an event, a structural fact.
2. **Behavioural errors.** A counterparty knows but reacts badly: overreaction, anchoring, herding, panic.
3. **Constraint errors.** A counterparty knows and reacts rationally, but is *forced* to trade at a bad price: margin calls, rebalancing, mandates, inventory limits, needing immediacy.

Each source shows up at different horizons. Constraint and behavioural errors dominate at short horizons (microstructure, flow). Information errors tend to play out at medium and long horizons (repricing on fundamentals, regime shifts).

## Two refinements

- **Errors are relative.** You don't need to be right. You only need to be *less wrong than the person on the other side*, by more than your costs and your own price impact. That means edge isn't absolute. It's a function of your counterparties, which closes the loop back to "it depends on your audience."
- **You are part of the market.** In the monopoly and thin-market cases, your own trading moves the exit price. Strictly, the question isn't "is this price good versus fair value?" It's "is this price good versus the price I can *exit* at, given that I'm the one exiting?"

## As a single expression

$$
\text{Edge} \approx \underbrace{\mathbb{E}[\text{exit price} \mid \text{future counterparties}] - \text{entry price}}_{\text{gap you capture}}  - \underbrace{\text{impact}(q, \text{depth})}_{\text{your own footprint}} - \text{costs}
$$

Here the expected exit price depends on interest (is anyone there?) and competition (how much of the gap survives others getting to it first?).

If you want formal anchors for this, two classic models from market microstructure each formalise part of it. Kyle (1985) covers informed traders versus noise traders and how price impact depends on depth. Glosten–Milgrom (1985) explains why spreads exist when some counterparties know more than others. Your framing essentially generalises both by letting participation and competition vary.


*Note: We are therefore looking to hit these ideas best*\

reference   v[i,t] = a[i] + b[i]·F[t]              b from a trailing window\
deviation   d[i,t] = p[i,t] − v[i,t]               scaled by its own trailing volatility\
state       s[t]                                   two or three states at most\
forecast    E[r(i, t→t+h)] = b[i]·E[r(F) | s] − (1 − exp(−κ(s)·h))·d[i,t]

# Our idea

with the above as our premise, we have the following conditions.
1. We must be able to get in and out of a trade, meaning we do not want to be in illiquid markets.
2. 

We ought to earn money because of:
1. We utilise market "inevitabilities" to earn money on holding, buy, selling between events that are bound to happen.
2. We utilise market "inefficiencies" to earn money on minimising errors in the markets.
3. We utilise market "matching" to earn money on providing liquidity


## market inevitabilities
Here we are considering things that WILL happen in the markets at some point, regardless of how much we try to reduce errors in prices.

## market inefficiencies
- price discrepancies in currency pairs
- price discrepancies in lead-lag between currencies
- price discrepancies in venue-lag
- relative value discrepancy within an asset
- price discrepancy based on information
- liquidity gaps

## market matching
market making, 



