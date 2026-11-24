
## Volume

### Baseline volume ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| V-01 | Volume is roughly lognormal distributed | . | Partially Supported | expected true | . |
| V-02 | Volume is mean reverting | . | Partly supported (reverts to a moving mean only) | expected vague | . |
| V-03 | Volume is has long memory? | . | Supported | expected vague | . |
| V-04 | Periods of lower than usual volume follows periods of higher than usual volume and vice versa | . | Not supported (volume regimes persist) | expected true | . |
| V-05 | One can with reasonable certainty estimate a "half-life" or "time until" next high volume period | . | Not supported (close to memoryless / clustered) | expected true | . |
| V-06 | One can with reasonable certainty estimate a "half-life" or "longevity expectation" for a high volume period | . | Partly supported (half-life estimable, durations dispersed) | expected true | . |
| V-07 | The volume mean drifts over time | . | Supported | expected true | . |
| V-08 | Periods with increasing volume mean have more positive returns than negative returns? | . | Not supported | unknown | . |
| V-09 | Periods with decreasing volume mean have more negative returns than positive returns? | . | Not supported | unknown | . |
| V-10 | Low volume goes with negative returns | . | Not supported | expected vague or untrue | . |
| V-11 | High volume goes with high absolute return | . | Supported | expected true | .|
| V-12 | Low volume goes with low absolute return | . | Supported | expected true | . |
| V-13 | Given the previous 2 years mean volume as the current expected mean volume, if the current month/year has lower than expected mean volume, the returns in this period should have a smaller absolute size than the returns in the previous years with higher volume. | . | Supported | unknown | . |
| V-14 | Given the previous 2 years mean volume as the current expected mean volume, if the current month/year has lower than expected mean volume, the rolling relative volatility of prices should be higher than in the previous period. The direction of prices over the period should also be more "stable", as in going in the same downwards or upwards direction but with jumps and shakes rather than swings. | . | Not Supported | unknown | . |
| V-15 | If the notional volume is decreasing over time, does the absolute return sizes decrease? | . | . |  |  |
| V-16 | Corrected for dollar volume, does lower dollar volume periods mean higher likelihood of negative returns? | . | . | . | . |

### Rolling volume ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| RV-01 | Rolling sum of volume is lognormal distributed | . | . | expected true | . |
| RV-02 | Rolling mean of volume is also lognormal distributed | . | . | expected true | . |
| RV-03 | Rolling sum of volume can help predict the future realised volatility | . | . | expected true | . |
| RV-04 | Rolling mean of volume can help predict the future realised volatility | . | . | expected true | . |
| RV-05 | if the current volume is above the rolling mean of volume, it correlates with a higher than usual realised volatility | . | . | expected true | . |
| RV-06 | if the current volume is below the rolling mean of volume, it correlates with a lower than usual realised volatility | . | . | expected true | . |
| RV-07 | if the current rolling mean of volume is significantly higher than the expected rolling mean of volume, than a period of below the expected rolling mean of volume should occur after. | . | . | expected true | . |
| RV-08 | the rolling volume profile can help understand trends and reversions | . | . | expected too vague | . |
| RV-09 | A range say something like 10 out of 15( 20 out of 30, etc.) of "below average" volume indicates a period of lower than usual absolute returns | . | . | . | . |

### Time volume ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VT-01 | Volume is seasonal within the day and the week |  |  |  |
| VT-02 | US Open hour has a significant increase in volume |  |  |  |
| VT-03 | US Close hour has a significant decrease in volume |  |  |  |
| VT-04 | Weekends have significantly lower volume |  |  |  |
| VT-05 | nights have significantly lower volume|  |  |  |
| VT-06 | Corrected for dollar volume, is the volume going up or down over time? | . | . | . | . |

### Volume price memory ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VP-01 | High-volume prices are revisited | . | . | . |
| VP-02 | A large negative return occurring with low volume will have the price return to the pre-jump price within 3 days | . | . | . | . |
| VP-03 | A price range(say central price +/- 1%), with an exceptionally higher than usual rolling mean of volume and rolling sum of value. are more likely to be revisited within 3 days. | . | . | . |
| VP-04 | A sudden jump or drop in prices with a significantly lower than usual rolling mean and rolling sum of volume, is more than 50% likely to give back part of the jump or drop within 1 hour. | . | . | . |
| VP-10 | A high-volume tight range(returns over the range staying within +/-0.5% from the start price of the range) has a higher than 50% chance of being followed by a low-volume drop or low-volume jump. Say max of 30 minute windows for the range.  | . | . | . | . |
| VP-11 | Absorption bars (very high volume, small move) that conclude with a jump or drop on high volume, have a higher than 50% chance of continuing. Say absorption bars can be up to the 30 minute window again. | . | . | . | . |
| VP-12 | Yesterday's point of control(very heavy volume, price area) is a magnet today? | . | . | . | . |
| VP-12 | Yesterday's point of control(very heavy volume, price area) is ONLY a magnet today if today's volume is below average | . | . | . | . |
| VP-13 | Fair-value-gap midpoints get filled, low-volume gaps more often | . | . | . | . |
| VP-14 | Busy price areas (30-day volume at price) are revisited more than quiet ones at the same distance | . | . | . | . |
| VP-15 | volume weighted moving average prices generally have lower error from realised prices than simple moving averages| . | . | . | . |
| VP-16 | the rolling(window 30 minutes) of volume * return indicator predicts direction as "future direction is the same as sign of the indicator", does this work for 1,5,10 minutes ahead ? | . | . | . | . |
| VP-17 | the sign of the (rolling(window 30 minutes) of volume * return - moving average of the same) indicator predicts direction as "future direction is the same as sign of the indicator", does this work for 1,5,10 minutes ahead ? | . | . | . | . |
| VP-18 | sign(Σ volume × return) predicts direction | . | . | . | . |
| VP-19 | An action principle for prices, with volume as mass or attention weight, along with returns as the returns on different points of a particle, can help predict direction of the next 1, 5, 10 minutes return. | . | . | . | . |
| VP-20 | When volume-weighted flow is balanced, price converges to VWAP | . | . | . | . |
| VP-21 | Net taker buying predicts the next return direction | . | . | . | . |
| VP-22 | When flow and price disagree, the next move follows the flow | . | . | . | . |

### Volume & Trend ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VTR-01 | Moves come with recognisable volume profiles before, during and after | . | . | . | . |
| VTR-02 | A downwards price trend is highly likely to have the "high, low, extra high" volume profile in first third, middle and last third of the trend.  | . | . | . |
| VTR-02 | An upwards price trend is highly likely to have either the "high, low, extra high" volume profile in first third, middle and last third of the trend. or the "high, higher, highest" volume profile. | . | . | . |

### Volume additional ideas
| ID | Hypothesis | Test | Result | Status | Source |
|---|---|---|---|---|---|
| VA-01 | 30-day breakouts continue more when volume is stronger than on the first leg, or ramping in | . | . | . | |
| VA-02 | high volume Downtrend or a high-volume jump down, and then any price behavior with ordinary volume, should lead to continuation | . | . | . | |
| VA-03 | high volume Downtrend or a high-volume jump down, and then any price behavior with high volume, should lead to reversal | . | . | . | |
| VA-04 | After a jump down, another one jump down is more likely than usual if the volume is significantly above average | . | . | . | . |
| VA-06 | a "normal distributed" volume distribution( as in increasing until the peak of the drop, decreasing after the drop returns normalise) with a drop in price "lands" where the volume drops down, the accumulation distribution indicator helps here | . | . | . | . |
| VA-07 | The slope of the Accumulation distribution indicator correlates with the returns. Low ACC DIST correlates with a drop in the current price to relative value. Meaning price will reverse | . | . | . | . |
| VA-08 | a range of continuously growing volumes, than 1 or 2 decreasing volume, than 1 growing is the sign of a "settled level". Meaning the prices will either increase/decrease until that decreasing volume and than reverse a bit on that next growing volume. | . | . | . | . |


