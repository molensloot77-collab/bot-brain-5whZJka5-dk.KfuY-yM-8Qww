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

## Skill-authoring

- **Shell heredocs inside skill markdown must not live inside list items.**
  Markdown list indentation (3-space) corrupts heredoc termination (closing
  `PY` with leading spaces doesn't terminate) and breaks embedded Python
  (IndentationError at module level from the list prefix). Keep code fences
  at column 0; reference them from the list. Surfaced 2026-04-18 on
  morning-check v2026-04-18b.
- **Verify every embedded code block on first write.** If a SKILL.md
  contains embedded Python or shell, run a one-shot parse check before
  trusting the file. Compile-check Python with `compile(code, '<name>', 'exec')`;
  dry-run shell with `bash -n`.
- **Real data invalidates proposed output formats.** First run of
  morning-check showed the output template was anchored on a non-existent
  `paper_settled.jsonl`. Always trace the skill's data assumptions to real
  files in the first run, not first-draft review. CopyBot settled lives
  inline in `sports_signals.jsonl` as RESOLVED events; no separate file.
- **Let Claude Code's improvisation teach the skill.** When a skill
  underspecifies a data source and Claude Code finds a better one at
  runtime (e.g. NewsBot `gate_status.json` over raw signals.jsonl grep),
  promote the discovery into the next skill version rather than leaving
  the skill to re-discover on every run.

## Pipeline observations

- **ScoutBot recycles falsified categories.** First strategic-review run
  (2026-04-19) showed 14+ PolyArb-mechanism items still being promoted
  at scores 7.0-8.8, plus Kalshi crypto-up/down integrations and
  market-making variants. The strategic-review skill's FALSIFIED.md
  cross-reference correctly skipped them, but their continued generation
  means ScoutBot's hypothesis space is too narrow — it lacks awareness
  of which categories have been killed. Layer 0 (idea generation) needs
  work alongside Layer 1 (triage).

- **Future skill candidate: scout-quality-audit.** Measures what % of
  SCT-AUTO items per week would be killed by FALSIFIED.md cross-reference,
  and surfaces the specific falsified categories ScoutBot keeps revisiting.
  Output: a brief weekly note like "this week ScoutBot promoted 18 items
  in already-falsified PolyArb-mechanism category — consider adding
  category-blacklist to ScoutBot scoring." Build only if 3+ weekly
  strategic-review runs continue showing >20% kill rate.

- **Pipeline-progression discipline (from strategic-review build, 2026-04-19):**
  Build Layer 1 (triage) before Layer 2 (backtest) before Layer 3
  (gate). Each layer's output must prove useful for 2-3 cycles before
  building the next layer. Resist the holy-grail "one prompt does
  everything" temptation — pipelines that promise to find/test/decide
  autonomously produce confident-sounding outputs that are wrong.
