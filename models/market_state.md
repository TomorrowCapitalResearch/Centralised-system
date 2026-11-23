Market states
Time state
asset longevity(how many days has the asset been public for?)
Season (what is the month? is it in the winter, summer, spring, fall?)
Day of month
Day of week
Hour of day
US open hour?
quiet hours? weekend or etc?
Volume state
the current rolling average volume
is the current rolling average moving upwards over time or downwards over time?
is the current dollar volume moving up or down?
is the current tick volume above or below expectation?
does the current period of volume match one of the volume profiles?
volume in the top Q or bottom Q, is the abs return matching expectation?
is this volume tick an outlier?
Volume seismics
dunno yet
Market cap state
current market cap(all time high)
current time since last all time high
current lowest low since last all time high
current time since last lowest low since last all time high
did we reach a swing, at least 10% up, since last lowest low? or did we simply continue down?
How many % were the last market cap above its last market cap?
the zigzag pattern between ATH, ATL, GH, GL, LH, LL.
Intraday range state
are open, high, low, close all moving in the same direction?
is the distance between High, low, close and open getting smaller or larger?
are the distances above or below their rolling norm?
is the low to close getting smaller and the high to close getting bigger?
is any of the distances abnormal?
how do the above statements look for a window 10, 20, 30 minutes?
Sign patterns?
up, up, up, ... minute by minute 5 mins by 5 mins? ...

returns?
volatility?
volume state rolling mean volume for the regime current volume profile - window adjustable? higher or lower than usual volume?

intraday range profiles

returns profiles

volatility profiles

trend profiles

moving average profiles

cross-asset profiles

lead-lag

correlations

...

ohlc

traight line passive MM, active MM.

Passive market making
Lead Lag
State space match
relative value arbitrage
high mean reversion
high trend
high jump
reference v[i,t] = a[i] + b[i]·F[t] b from a trailing window
deviation d[i,t] = p[i,t] − v[i,t] scaled by its own trailing volatility
state s[t] two or three states at most
forecast E[r(i, t→t+h)] = b[i]·E[r(F) | s] − (1 − exp(−κ(s)·h))·d[i,t]
