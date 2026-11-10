






























our fundamental hypothesis.\
the markets is a dynamic system continuously matching supply and demand in tradeoffs between costs, believed price, spontaneous supply-demand and dynamic evolving behaviors.\
this means that forecasting supply and demand and their effects on the prices is the way to predict best trades.\


## mass/volume
it is our belief that "volume" can not be directly associated with mass, but that it can at the very least hint at the amount of attention or mass associated with a price or move.
It is this associated belief that leads us to the following tests.



- we first want to know mean, std, skew, kurtosis, distributions.

Most are very close to normal distributed under a log transformation. Raw volume is seemingly exponential decay distributed.

--- Volume Statistics Summary ---
                    mean           std       skew     kurtosis      count
BTCUSDT     4.630529e+01  9.491072e+01   8.299822   141.600483  3057120.0
ETHUSDT     3.842582e+02  7.059713e+02   9.093398   169.586167  3057120.0
DOGEUSDT    1.369108e+06  5.155950e+06  23.437017  1024.306773  3057120.0
SOLUSDT     3.012913e+03  5.849953e+03  12.318459   407.435329  3057120.0
EURUSD_ask  1.421302e+02  1.646625e+02   3.217866    18.133943  1392696.0
EURUSD_bid  1.504667e+02  1.729201e+02   3.311658    18.369071  1361174.0
XAUUSD_ask  3.217810e-02  3.291596e-02   4.034834    90.565947  1326991.0
XAUUSD_bid  3.220747e-02  3.281557e-02   3.094181    27.159213  1326991.0

--- Log-Transformed Volume Statistics Summary ---
                 mean       std      skew   kurtosis      count
BTCUSDT      2.996372  1.260535  0.327483  -0.252827  3057120.0
ETHUSDT      5.261503  1.158454  0.014551   0.335961  3057120.0
DOGEUSDT    12.907991  1.552121 -0.387673   2.822809  3057120.0
SOLUSDT      7.277302  1.228253 -0.313241   1.491597  3057120.0
EURUSD_ask   4.412171  1.145742 -0.542708   0.468476  1392696.0
EURUSD_bid   4.482432  1.136569 -0.604217   0.650510  1361174.0
XAUUSD_ask   0.031195  0.030413  2.782706  19.612406  1326991.0
XAUUSD_bid   0.031222  0.030480  2.611507  12.448130  1326991.0



- *note: we wait with this* - Than we want to consider what happens if we "skip" the bottom 5% or some other threshold.\
- after this we want to check if volume is mean reverting. \

 
- than we want to check if the volume means move over time \


- than we want to check if the volume is trend \

such that the mean reversion happens around different means.\
than we want to know if big or small volume is associated with big or small returns \
than we want to look for volume profiles in "flat", "up", "down", "volatile",... periods \
does US open have higher volume or a volume jump?
does beginning/end or a specific day of the month have more activity?
Is there generally more or less activity in any season?


Check if "high volume" prices or price zones return within 10 days \
check if "low volume & drop" prices or price zones return within 10 days.



```text
=== BTCUSDT Volume / Activity Statistics ===
  Raw Volume -> Mean: 46.31 | 5th Percentile: 1.88
  Log Volume -> Mean: 2.9964 | 5th Percentile: 1.0581
[ETHUSDT] Using column 'volume' for volume/activity analysis.

=== ETHUSDT Volume / Activity Statistics ===
  Raw Volume -> Mean: 384.26 | 5th Percentile: 28.19
  Log Volume -> Mean: 5.2615 | 5th Percentile: 3.3739
[DOGEUSDT] Using column 'volume' for volume/activity analysis.

=== DOGEUSDT Volume / Activity Statistics ===
  Raw Volume -> Mean: 1,369,108.26 | 5th Percentile: 32,274.00
  Log Volume -> Mean: 12.9080 | 5th Percentile: 10.3820
[SOLUSDT] Using column 'volume' for volume/activity analysis.

=== SOLUSDT Volume / Activity Statistics ===
  Raw Volume -> Mean: 3,012.91 | 5th Percentile: 201.14
  Log Volume -> Mean: 7.2773 | 5th Percentile: 5.3090
[EURUSD_ask] Using column 'volume' for volume/activity analysis.

=== EURUSD_ask Volume / Activity Statistics ===
  Raw Volume -> Mean: 142.13 | 5th Percentile: 9.09
  Log Volume -> Mean: 4.4122 | 5th Percentile: 2.3115
[EURUSD_bid] Using column 'volume' for volume/activity analysis.

=== EURUSD_bid Volume / Activity Statistics ===
  Raw Volume -> Mean: 150.47 | 5th Percentile: 9.90
  Log Volume -> Mean: 4.4824 | 5th Percentile: 2.3888
[XAUUSD_ask] Using column 'volume' for volume/activity analysis.

=== XAUUSD_ask Volume / Activity Statistics ===
  Raw Volume -> Mean: 0.03 | 5th Percentile: 0.00
  Log Volume -> Mean: 0.0312 | 5th Percentile: 0.0035
[XAUUSD_bid] Using column 'volume' for volume/activity analysis.

=== XAUUSD_bid Volume / Activity Statistics ===
  Raw Volume -> Mean: 0.03 | 5th Percentile: 0.00
  Log Volume -> Mean: 0.0312 | 5th Percentile: 0.0035
=== BTCUSDT Bucket Analysis ===
  Total Observations: 3,057,120
  5th Percentile Value: 1.88
  Count <= 5th Percentile: 152,857
  Empirical Share: 5.00%

=== ETHUSDT Bucket Analysis ===
  Total Observations: 3,057,120
  5th Percentile Value: 28.19
  Count <= 5th Percentile: 152,856
  Empirical Share: 5.00%

=== DOGEUSDT Bucket Analysis ===
  Total Observations: 3,057,120
  5th Percentile Value: 32,274.00
  Count <= 5th Percentile: 152,865
  Empirical Share: 5.00%

=== SOLUSDT Bucket Analysis ===
  Total Observations: 3,057,120
  5th Percentile Value: 201.14
  Count <= 5th Percentile: 152,861
  Empirical Share: 5.00%

=== EURUSD_ask Bucket Analysis ===
  Total Observations: 1,392,696
  5th Percentile Value: 9.09
  Count <= 5th Percentile: 69,845
  Empirical Share: 5.02%

=== EURUSD_bid Bucket Analysis ===
  Total Observations: 1,361,174
  5th Percentile Value: 9.90
  Count <= 5th Percentile: 69,025
  Empirical Share: 5.07%

=== XAUUSD_ask Bucket Analysis ===
  Total Observations: 1,326,991
  5th Percentile Value: 0.00
  Count <= 5th Percentile: 66,370
  Empirical Share: 5.00%

=== XAUUSD_bid Bucket Analysis ===
  Total Observations: 1,326,991
  5th Percentile Value: 0.00
  Count <= 5th Percentile: 67,275
  Empirical Share: 5.07%
```

**Event-clock**\
The event clock added to the asset data table, is therefore the jump in timesteps until an even is above the bottom 5% volume.

we can see there are no 0-volume timesteps.

<img width="934" height="604" alt="image" src="https://github.com/user-attachments/assets/e5961e53-4e09-4bac-97f6-45d7f248bef9" />
<img width="916" height="612" alt="image" src="https://github.com/user-attachments/assets/c2d1087f-ad65-4ca2-80c3-78d8c4453366" />
<img width="888" height="608" alt="image" src="https://github.com/user-attachments/assets/cfa03ce8-df23-454b-bccd-f9c9c8e554ad" />
<img width="888" height="617" alt="image" src="https://github.com/user-attachments/assets/5c053ba4-2d33-43c8-805b-883beef5725d" />



## velocity / returns

It is our goal to understand how returns behave. Imagining returns to be velocities of movement of a particle, we set out to do the following.\
A stream of particles, will have an say 4 outermost points of the beam, we are first attempting to understand the movements of those 4 points.\



we first want to find correlations between their returns, just on the same timestep \
than we want to find autocorrelations and check correlations to spreads \





| Type | mean | std | skew | kurtosis | min | max |
|---|---|---|---|---|---|---|
|Open| 0.000007 | 0.000650 | 0.520842 | 4.149302 | -0.002593 | 0.004404 |
|High| 0.000007 | 0.000606 | 1.130605 | 6.945386 | -0.002142 | 0.004602 |
|Low| 0.000007 | 0.000576 | 0.357939 | 5.367008 | -0.002647 | 0.004297 |
|Close| 0.000007 | 0.000650 | 0.520512 | 4.147852 | -0.002593 | 0.004404 |
|High Low| 5.857797 | 107.161273 | 20.734450 | 447.169557 | -0.999797 | 2553.999998 |
|Open Close| -15.047907 | 254.229297 | -23.887206 | 641.408332 | -7160.999993 | 514.200000 |
|High Close| 144.285734 | 861.055685 | 9.071374 | 106.217630 | -1.000000 | 13866.999987 |
|Close Low| 312.557159 | 1407.023330 | 6.916645 | 60.989254 | -1.000000 | 18235.000010 |

<img width="1083" height="711" alt="image" src="https://github.com/user-attachments/assets/52ce2ea2-013b-44e4-8ce4-224775221992" />

<img width="790" height="637" alt="image" src="https://github.com/user-attachments/assets/427daf03-4af5-47f5-9ad0-2aa8ceb601b0" />










## turbulence/volatility
we want to understand the general turbulence behaviors, this we plan to utilise for "market limit orders" for various different periods.
If we can predict the likely swings in the prices, open, high, low, close, we might be able to also predict likely zones for hits on limit orders, such that
We can take advantage of those on profitable trading strategies.

### initial plots
**standard price column volatilies**\
These were built on the open, high, low, close prices.\
<img width="1171" height="470" alt="image" src="https://github.com/user-attachments/assets/8e074a84-98a4-44e5-9fa4-0d5a482fe11d" />

**ranges volatilities**\
this plot was run on a short horizon, for the spreads we developed earlier, high-low, close-low, high-close, close-open.\
<img width="1165" height="471" alt="image" src="https://github.com/user-attachments/assets/b09880eb-ea4c-4648-a008-5a4b8d35449c" />

### longer horizon plots

these were done on prices, Open high low close. As well as on the price returns.

<img width="1055" height="310" alt="image" src="https://github.com/user-attachments/assets/2f200e38-6b09-4d5a-a165-574704e761ee" />

<img width="1055" height="310" alt="image" src="https://github.com/user-attachments/assets/e4654fa2-7384-4150-a942-96ac29d49ef7" />





## simple markov states.

**TOP PATTERNS FOR LENGTH = 3** \
pattern_3  frequency prob_str deviation_str \
      udd        129    55.0%        +0.050 \
      uuu        115    47.0%        -0.030 \
      dud        125    47.2%        -0.028 \
      duu        128    47.7%        -0.023 \
      ddd        119    48.7%        -0.013 \
      udu        124    50.8%        +0.008 \
      uud        128    50.8%        +0.008 \
      ddu        129    50.4%        +0.004 \

**TOP PATTERNS FOR LENGTH = 5** \
pattern_5  frequency prob_str deviation_str \
    duduu         28    32.1%        -0.179 \
    uduud         34    64.7%        +0.147 \
    uuudd         33    63.6%        +0.136 \
    duudd         30    63.3%        +0.133 \
    duuud         32    37.5%        -0.125 \
    uuddd         23    60.9%        +0.109 \
    ddddu         27    59.3%        +0.093 \
    uduuu         29    41.4%        -0.086 \
    udduu         35    42.9%        -0.071 \
    uuduu         35    57.1%        +0.071 \

**TOP PATTERNS FOR LENGTH = 7** \
pattern_7  frequency prob_str deviation_str \
  uuddudd         11    81.8%        +0.318 \
  uududdu         10    80.0%        +0.300 \
  uduudud         10    20.0%        -0.300 \
  uduuudd         12    75.0%        +0.250 \
  uduuduu         12    25.0%        -0.250 \
  ududduu         11    27.3%        -0.227 \
  uuduuud         11    27.3%        -0.227 \
  duududd         11    72.7%        +0.227 \
  ddududd         11    27.3%        -0.227 \
  udddudu         10    30.0%        -0.200 \


