# Falsified Theses

Strategies, signals, or architectural assumptions that have been tested and failed.
Purpose: prevent re-litigation. Before proposing anything that rhymes with one of
these, read the relevant entry and confirm why the conditions have materially changed.

---

## PolyArb: Chainlink oracle lag arbitrage

**Hypothesis:** Polymarket's resolution oracle (Chainlink Data Streams) lags Binance spot prices by ≥2 seconds, enabling arb between the true price and the crypto up/down market price before resolution.

**Test:** ARB-59, commit `76d7e93`. Logged `oracle_lag.jsonl` with `wall_lag_ms`, `oracle_vs_binance_ms`, `chainlink_round_id` across BTC/ETH/SOL/XRP over multiple days.

**Result:** Median lag **negative** (Chainlink leads Binance) for all four assets. Price divergence at resolution time averaged 0.02% vs a 1% fee hurdle.

**Confidence:** High. Multiple weeks of data, multiple assets, consistent signal.

**Corollary:** Chainlink RTDS streams at ~1Hz. No discrete timing edge to exploit. KrajekBot Path 1 (oracle timing edge) killed on the same evidence.

---

## PolyArb: Pair-sum arbitrage

**Hypothesis:** Pairs of inversely-correlated Polymarket binary markets (YES+NO across related contracts) should sum to exactly $1.00. Deviations below create risk-free arb.

**Test:** ARB-83, commit `67e9e76`. Scanned 187,856 pair observations.

**Result:** Zero pairs observed below $1.00. Book is efficient on this dimension.

**Confidence:** High. n=187,856 is decisive.

---

## PolyArb: FIFO maker rebate harvesting

**Hypothesis:** Placing maker orders at $0.40 on liquid crypto markets captures rebate with acceptable fill rate.

**Test:** ARB-62. Measured fill rate at $0.40 across multiple sessions.

**Result:** 0.09% fill rate. Book pinned at $0.51 in 93% of measured windows.

**Confidence:** High. Market microstructure is the problem, not execution.

---

## CopyBot: Polymarket Gamma API as settlement source

**Hypothesis:** Gamma `outcomePrices` field is authoritative for market resolution.

**Test:** Apr 3–4, audit comparing Gamma vs CLOB settlement status across multiple markets. Multiple "WIN" entries from Gamma that CLOB showed as open or non-resolved.

**Result:** Gamma returns false positives. CLOB book+LTP is authoritative.

**Confidence:** High. Pattern confirmed across multiple audits.

**Corollary:** RULE 21 codified — CLOB-first settlement, never Gamma. `harvester_outcomes.py` cross-checks CLOB before trusting Gamma.

---

## KrajekBot: Phase 2 backtest results (pre-forward-only)

**Hypothesis:** KrajekBot v4.x shows 60%+ WR in backtest across BTC/ETH/SOL on 15-min windows.

**Test:** Attempted live deployment Mar 2026. 52 live trades, 48.1% WR vs 62.8% breakeven.

**Result:** Lookback artifact. Entry signal was captured at minute 5 of a 15-minute window but had already observed minute 0–5 outcome data. WR inflated monotonically with lookback depth.

**Confidence:** High. All Phase 2 backtests invalidated.

**Corollary:** Forward-only entry signal methodology mandatory. Entry must use only information available before window opens. Current re-paper phase (target 300+ v4.6 trades) is the re-validation.

---

## CopyBot: On-chain scorer as primary wallet signal

**Hypothesis:** Polygonscan transaction history is sufficient for wallet quality scoring.

**Test:** Multiple wallet cohorts scored high on-chain but produced zero copyable signals (e.g., avg entry 0.97+, 0 band-rate passes).

**Result:** On-chain scorer is blind to proxy/CTF wallet structure. Polymarket activity API is the authoritative signal. Polygonscan is a cross-check for capital movements, not a primary scorer.

**Confidence:** Medium-high. Multiple FAVORITE_BUYER cohorts confirm the pattern.

---

## Entire category: BTC/ETH up/down markets

**Hypothesis:** Short-interval crypto up/down markets have exploitable edge at retail scale.

**Test:** PolyArb (3 theses above) + KrajekBot (crypto TA).

**Result:** Markets are too efficient. 10% taker fee on this category makes the hurdle prohibitive. Oracle is tight. Book is tight. No retail-scale edge observed across 6+ weeks.

**Confidence:** High for PolyArb microstructure. Medium for TA (KrajekBot KR-FUND signal still under test, but rare/regime-dependent).

**Policy:** No new strategies on this market category without a specific, testable mechanism that differs from oracle timing, pair-sum, maker-rebate, or TA.

---

## KrajekBot v4.6 re-paper + KR-FUND regime gate

**Hypothesis (KR-FUND):** BTC SHORT signal during CROWD_SHORT funding regime would generate 73.5% WR observed in n=34 corpus retest.

**Test:** Re-paper Apr 12-19 (7 days, 52 trades), regime monitoring continuous (~509,000 regime evaluations).

**Result:** Zero CROWD_SHORT regime occurrences in 7 days. Signal cannot have fired. Base re-paper strategy (without KR-FUND): 48.1% WR, -$5.37 P&L.

**Confidence:** High. The regime monitoring is independent of the trade signal — its absence is empirical, not computational.

**Corollary:** Combined with PolyArb's three falsifications (oracle lag, pair-sum, FIFO rebate), the entire crypto up/down market category at retail scale on Polymarket is falsified across 4 distinct mechanisms tested. Future strategies on this category require materially new hypothesis types — not refinements of TA, oracle-timing, or microstructure approaches already tested.
