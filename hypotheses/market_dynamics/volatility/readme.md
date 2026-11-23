## 7. Volatility

| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VO-01 | In some periods the highs are the more volatile series (sellers coming down to meet price), in others the lows (buyers coming up) | Open slot. `vt.volatility_features` gives the rolling volatility of O, H, L, C | Not tested. | ⏳ | |
| VO-02 | Volatility is cyclical: low to high and back | Open slot | The persistence half holds (PS-01, PS-03). Cycle length, and whether the position in the cycle says anything about the next move, are not tested. | 🟡 | NB |
| VO-03 | A random forest can call the direction of rolling volatility | Forest against the majority class, a one-line window rule and shuffled bars; 80 / 20 split in time order | No result on file. The earlier cell was interrupted. | ⏳ | |
