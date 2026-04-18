# Workspace — Live Task State

Last updated: 2026-04-18 (Saturday)
Updated by: Claude Chat (migration session)

## Current focus

**CB-DECAY-MONITOR gate — Apr 18 evening.** Volume question resolved:
516 PAPER fills in the last 24h alone demonstrates the surviving wallet
pool is generating more than enough signal throughput post-cull. Gate
question narrows to: **is the 63% WR from Apr 17 (194W/112L, n=306)
holding on post-Apr-17 data, or has it regressed?**

As of Apr 18 10:48 UTC: 413 fills / 120 settled / 68W 52L / WR 56.7% in the
trailing 24h. That's a meaningful drop from 63% and n=120 is enough to take
seriously but not enough to be decisive. Decision: let the 02:00 UTC
harvester run tonight, then evaluate the full Apr 17–18 post-cull cohort
against the pre-cull top-20 restore candidates. Gate eval session tonight
or tomorrow morning depending on data freshness.

## Open tasks by bot

### CopyBot
- [ ] Evaluate CB-DECAY-MONITOR gate — post 02:00 UTC harvester, Apr 18 evening or Apr 19 morning
- [ ] Sunday 06:00 UTC auto-rescore applies corrected BUY+SELL pair metric to all 809 T2 wallets (CB-WATCHLIST-STALE fix)
- [ ] CB-DEDUP-REPEAT open (MED priority) — watch for 0xafbacaeeda-style repeats

### KrajekBot
- [ ] Apr 23 gate (or May 7 if regime check fails): KR-FUND live deployment decision
- [ ] Regime check: if <10% of days show CROWD_SHORT, extend gate to May 7
- [ ] Post-re-paper: KR-STAGE1-FILTER remediation (SHADOW_LATE masks Stage-2 skips)

### NewsBot
- [ ] May 7 extended go/no-go gate (was Apr 22 — extended due to 0 settled trades)
- [ ] Fuzzy dedup fix (9.8% duplicate leak)
- [ ] 30s burst debounce (48% calls <10s apart)
- [ ] `datetime.utcnow()` deprecation fix in `pipeline.py:78`

### WeatherBot
- [ ] Apr 22 WB-RESUME go/no-go
- [ ] Apr 22 WB-29 city divergence prior gate
- [ ] WB-30 (paper_settler fix) — first task next WeatherBot session

### ScoutBot
- [ ] SCT-2 Reddit API approval still pending (submitted Mar 30)
- [ ] Periodic bulk retirement of PolyArb-tagged SCT-AUTO items (~weekly)

### Cross-cutting
- [ ] Polymarket exchange migration monitoring (@PolymarketDevs) — CTF allowance re-approval needed post-migration on all bot wallets
- [ ] SX Bet integration (CopyBot) — `sx_paper.jsonl` logging wired; review ~monthly

## Recent decisions (last 7 days)

- 2026-04-17: CopyBot switched to 100% paper mode via systemd drop-in; 306 settled trades confirm wallet-selection cull was wrong
- 2026-04-16: CB-NOISE-PAPER-BUG fixed (commit e60221f) — `bet_size = min(PAPER_BET_SIZE_USD, bet_size)`
- 2026-04-15: CB-SIZING-FIX — 148/157 T2 wallets raised from stale $0.50 to $1.00 MIN_LIVE_BET_USD
- 2026-04-13: CB-ASK-SORT critical bug fixed (commit 1e922c4) — `get_best_ask()` was returning worst ask for 19 days; 9 signals fired within 1 minute post-fix

## Blocked / waiting

- NewsBot gate: blocked on Polymarket resolution lag (0/121 settled, Apr 30 cohort is first realistic window)
- WeatherBot WB-RESUME: blocked until 14-day WB-28 verification completes (~Apr 22)
- KrajekBot KR-FUND: blocked until Apr 23 regime check

## Session conventions

- Morning: read this file first, then run morning check skill
- After any significant action: update "Open tasks" and "Recent decisions" here
- Evening: append to `/root/.agent/memory/episodic/session_log.jsonl`, commit
