# Derivatives, positioning and external events

[Category index](../START_HERE.md) · [Source guide](../source_index/Sources.md)

**Does information outside the spot bars change the forecast?** Open interest, funding, quote-currency moves and dated macro surprises.

The attached alpha register specifies an open-interest-conditioned drop and a funding-extreme rule as untested. They need timestamped derivatives data. The FX manual describes construction of cross rates and executable bid/offer sides. (Alpha M4–M5, FX manual “Cross Rates”, p. 22.)

**Test design.** Record first public availability, funding accrual intervals and event timestamps. Historical forecast tests need the values visible at that time, including the contemporary consensus for a surprise.

### Proposed tests

Each row is open. The data column identifies the inputs needed for the comparison.

| ID and origin | Hypothesis | Decisive comparison | Data | Source and related IDs |
| --- | --- | --- | --- | --- |
| <a id="r-ext-01"></a>**EXT-01** · From research notes | For equal spot drops and notional volume, a concurrent decline in perpetual open interest changes the probability of a 50% retracement. | Freeze OI values at their actual publication times. Compare falling vs stable OI after matched 3σ five-minute drops. Score 4 h retracement vs extension vs neither, controlling basis, funding and BTC. A forced-liquidation interpretation needs liquidation records. | Spot bars, perpetual OI, basis. Liquidation feed for mechanism attribution | Alpha M4 and Framework FND-016. [JMP-04](08_jumps_precursors_and_recovery.md#r-jmp-04), [VA-02](08_jumps_precursors_and_recovery.md#r-va-02), [VA-03](08_jumps_precursors_and_recovery.md#r-va-03) |
| <a id="r-ext-02"></a>**EXT-02** · New question | Some apparent BTC/USDT jumps reflect a USDT repricing and largely disappear in a common dollar numeraire. | Declare an event from BTC/USDT alone. At synchronised times compare BTC/USD and BTC/USDT converted at an independent USDT/USD mid. Attribute returns to BTC and quote currency. Contrast with matched ordinary dates and a second quote currency. | BTC/USD, BTC/USDT and independent USDT/USD quotes | New numeraire-attribution test informed by Alpha M2/M3. [RVL-03](11_relative_value_basis_and_venue_leadership.md#r-rvl-03), [JMP-03](08_jumps_precursors_and_recovery.md#r-jmp-03) |
| <a id="r-ext-03"></a>**EXT-03** · New question | The size of a scheduled macroeconomic surprise explains crypto shock risk around a release beyond ordinary clock-time seasonality. | Compare absolute actual-minus-consensus surprise available at release with next-5/30-min absolute returns. Use matched non-release dates at the same NY time, separate release types and overlap with the cash open, and cluster by release date. | Timestamped releases, unrevised values, contemporaneous consensus, minute bars | New external-event comparison informed by T-03 and the macro notes. [T-03](01_calendar_sessions_and_asset_age.md#r-t-03), [CAL-02](01_calendar_sessions_and_asset_age.md#r-cal-02) |
| <a id="r-ext-04"></a>**EXT-04** · From research notes | Extreme perpetual funding anticipates a change in basis or spot return after current basis and momentum are accounted for. | Freeze the announced rate at its publication time. Estimate next-8 h basis change and spot signed return as separate endpoints. Match time to the funding transfer, volatility and basis. Compare against a basis-only forecast. | Perpetual rate history, event timestamps and synchronized spot/perp quotes | Alpha M5 and the basis programme. [RVL-02](11_relative_value_basis_and_venue_leadership.md#r-rvl-02), [EXT-01](#r-ext-01) |
| <a id="r-ext-05"></a>**EXT-05** · New question | A larger preannounced token unlock relative to circulating supply is followed by weaker factor-adjusted returns than a smaller unlock. | Use the unlock schedule known seven days in advance and the circulating supply then observable. Compare next-24 h and 7-day residual returns across unlock sizes against matched non-unlock dates. Include the pre-event return as a separate outcome and cluster related releases. | Timestamped unlock schedules, revisions, historical float and token returns | New supply-side question. [RVL-01](11_relative_value_basis_and_venue_leadership.md#r-rvl-01), [EXT-03](#r-ext-03) |

### Strategy and model references

The following source IDs are indexed under this category in [Alpha_and_model_cards.csv](../source_index/Alpha_and_model_cards.csv).

ALPHA/M4, ALPHA/M5, ALPHA/LEGACY-N6, ALPHA/IND-05, ALPHA/IND-06, ALPHA/IND-19, ALPHA/IND-20, ALPHA/IND-22, ALPHA/E20, ALPHA/E21, ALPHA/VO-01, ALPHA/VO-02
