# Lessons — Distilled Patterns

Append-only. Read before non-trivial decisions. New lessons auto-promoted
from episodic memory by the evening dream cycle; manual additions clearly
tagged with date.

---

## Wallet quality (CopyBot-specific, but patterns generalize)

- Profiler must check average entry price. Wallets trading at 0.97–0.999 generate zero copyable signals regardless of on-chain WR.
- `FAVORITE_BUYER` gate: `avg_buy_price > 0.85 AND buy_count ≥ 10` triggers rejection.
- `price_band_rate` gate in harvester: reject wallets with <30% of trades in 0.30–0.90 band.
- On-chain scorer is blind to proxy/CTF wallets. Use Polymarket activity API as primary signal; cross-check large single-session outflows on Polygonscan.
- Polymarket leaderboard P&L is a volume/deposit metric, not a profitability signal (SCT-GATE-1).
- Niche specialists beat leaderboard-famous wallets. Anti-crowding controls: SCAN-7 weekly rescore, SCAN-11 crowding detector, MON-5 basket consensus.

## Fee structure (critical — has killed bots)

- Polymarket fees on **sports/esports/event markets: $0.00**.
- Polymarket fees on **crypto up/down markets: 10% taker**. This is what killed KrajekBot.
- Always verify fee regime before backtesting any new market category.

## P&L and metric integrity

- Dashboard and scoreboard must filter exclusively on `LIVE+FILLED`. `CROWDED_SKIP`, `PAPER`, `FAILED` contaminate WR.
- Shadow settlement is standalone, independent of live position workflow.
- `activity_monitor.check_pm_settlement()` owns live position settlement — NOT `harvester_outcomes.py`.
- Ground truth for P&L on PolyArb-era setups: `python3 /tmp/pnl_check.py`. Trust this over service metrics.
- When results surprise you, check metric pipeline before accepting the result.

## Backtest methodology

- **Forward-only entry signals are mandatory.** Any backtest where entry signal has access to data from after the window opens is invalidated. KrajekBot Phase 2 lost months of work to this: signal captured at minute 5 of 15-min window had already observed outcome data.
- Signal validation must clear both backtest cache AND live paper corpus before live deployment (KrajekBot rule).
- Paper gates are measurement instruments, not formalities. Don't skip them. Don't cheat them.

## Diagnose before scaling

- "Hold the target, diagnose first." If a strategy underperforms, figure out why before changing parameters.
- CopyBot cull post-mortem (Apr 16): 95.7% of pre-cull edge was in wallets the cull removed. The answer "wallet selection is broken" took ~5 sessions to surface because we kept tweaking the strategy instead of auditing the selection filter.
- Corollary: when a filter rejects ≥90% of signals, the filter is the hypothesis under test, not the strategy.

## Architecture rules

- Never inline `regime_detector.py` in KrajekBot. Breaks MACD persistent EMA state.
- Never use `pb` alias for KrajekBot. It controls polybot only.
- CopyBot MAX_PRICE=0.90, MIN_PRICE=0.30, MIN_LIVE_BET_USD=1.00, daily loss limit $20, kill switch at -$50 paper P&L.

## Session hygiene

- Session logs: produced by Claude Chat at explicit close, not deferred to `evening.sh`.
- NEVER ask to update handovers mid-session. Evening routine owns all handover updates.
- Every Claude Code session prompt includes Task 0: grep today's SCT-AUTO items.
- Code files delivered via `present_files` only. Session prompts as direct copy blocks.
