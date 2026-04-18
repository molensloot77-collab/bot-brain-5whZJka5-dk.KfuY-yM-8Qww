# Decisions — Architectural Choices + Rationale

Append-only. Each decision includes date, rationale, alternatives considered,
and status. Purpose: avoid re-litigating settled questions.

---

## 2026-04-17: CopyBot switched to 100% paper mode with flat $10 bet size

**Decision:** Bypass NOISE_BET filter for paper trades. All signals execute as paper at flat bet size.

**Rationale:** Need to measure whether the Apr 13 wallet cull selected wallets with genuine directional edge. Live mode blocks NOISE_BET signals at bet/vol<0.001, so we can't observe signal quality on surviving wallets. Paper at flat $10 answers the selection question fast (100+ fills in days).

**Alternatives considered:**
- Widen NOISE_BET threshold → doesn't answer the question, only stops penalizing low conviction
- Wait for more live fills at reduced sizing → too slow, doesn't unblock gate decision

**Status:** Active. Evaluation at Apr 18 evening gate (CB-DECAY-MONITOR).

---

## 2026-04-16: CopyBot `CB_MAX_DRAWDOWN_USD=500` via systemd drop-in

**Decision:** Raise drawdown cap from default $50 to $500 for paper mode only.

**Rationale:** Default $50 cap trips during paper data collection before enough fills accumulate for meaningful WR. Paper losses aren't real capital. Revert path preserved: `rm` drop-in → default $50 reapplies.

**Status:** Active until live mode resumption.

---

## 2026-04-13: PolyArb retired

**Decision:** Kill PolyArb entirely. No further capital, no trading logic edits.

**Rationale:** Three core theses falsified:
- Oracle lag arb (ARB-59, commit 76d7e93): median lag negative across BTC/ETH/SOL/XRP; price divergence 0.02% vs 1% fee hurdle
- Pair-sum arb (ARB-83, commit 67e9e76): 187,856 observations, zero pairs below $1.00
- FIFO maker rebate (ARB-62): 0.09% fill rate at $0.40 target, book pinned at $0.51 in 93% of windows

Crypto up/down markets too efficient for edge at this scale. Keep `/opt/polybot/` frozen as institutional record.

**Alternatives considered:** Continue logging to see if regime changes. Rejected — 4+ weeks of data is sufficient falsification.

**Status:** Permanent. See FALSIFIED.md for full detail.

---

## 2026-04-11: Handover docs as "reference snapshots," server as ground truth

**Decision:** `/mnt/project/*.md` handover files treated as lagging snapshots. `/root/docs/*_CURRENT.md` on server is ground truth.

**Rationale:** Claude Chat can see project files instantly. Claude Code sees server. Keeping both synchronized mid-session caused repeated drift. Evening routine owns the sync.

**Alternatives considered:**
- Skip project files entirely, git-only → too slow for chat context
- Real-time sync → too fragile, too many small edits

**Status:** Active.

---

## 2026-04-08: NewsBot paper gate, 7 metrics, $3/day kill switch

**Decision:** NewsBot runs paper with 7-metric gate before any live consideration.

**Rationale:** Classifier cost was unbounded before cap. Top-150 market cap + $3/day kill switch limits exposure to ~$0.35/day actual spend. 7 metrics ensure signal quality before live commitment.

**Status:** Active. Gate extended to May 7 due to Polymarket resolution lag (0/121 settled).

---

## 2026-04-05: CopyBot live precision fix (commits 6db757c, 8aad909)

**Decision:** Price rounded to 2dp, size rounded to 4dp before CLOB submission.

**Rationale:** LIVE ORDER FAILED errors traced to precision mismatch with CLOB requirements. First live fill (Athletic Club @ 0.50, +$2.50) confirmed fix.

**Status:** Active. All subsequent live fills have cleared precision checks.

---

## 2026-03-??: KrajekBot re-paper mandate

**Decision:** Halt KrajekBot live trading, return to paper with target of 300+ v4.6 trades before any live re-deployment decision.

**Rationale:** 52 live trades showed 48.1% WR vs 62.8% breakeven (10% fee model). Lookback artifact discovered: Phase 2 backtests used entry signal at minute 5 that had already observed outcome data. Forward-only methodology mandatory.

**Alternatives considered:** Reduce bet size and continue live. Rejected — methodology was the problem, not sizing.

**Status:** Active. KR-FUND signal shows promise (73.5% WR, n=34) but gated on regime availability.
