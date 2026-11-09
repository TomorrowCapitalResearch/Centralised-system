

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
