# Workspace — Live Task State

Last updated: 2026-04-20 (Monday)
Updated by: Claude Chat (evening close)

## Current focus

**CB-DECAY-MONITOR gate CLOSED — Apr 20.** 4 bleeders demoted (commit e6dbce3). Cohort analysis reframed gate as bi-modal (13 carriers / 14 bleeders on n>=5) not broad decay. Re-evaluate ~Apr 25 with fresh post-demote data. Tomorrow's focus: CopyBot development work (CB-EXIT-MIRROR or CB-SCORE-RESOLVED-ONLY), not reconciliation.

## Open tasks by bot

### CopyBot
- [ ] CB-WATCHLIST-DRIFT-AUDIT — investigate 2257->162 prune Apr 15-17, commit skip cause. Trigger: before next watchlist write.
- [ ] CB-MARTINGALE-WATCH-0x8a81855d — monitor for n>=50 or single-loss >$5. Current: 77.3% WR n=22 +$1.80 paper, on-chain flag -$32K Martingale.
- [ ] CB-DECAY-MONITOR re-eval — ~Apr 25 with fresh post-demote cohort
- [ ] CB-EXIT-MIRROR — measure source-wallet exit behavior, sequence after polling-fix 24-48h data
- [ ] CB-SCORE-RESOLVED-ONLY — audit + correct scoring to exclude unresolved-as-wins, sequence after polling-fix 48-72h data
- [ ] Sunday 06:00 UTC auto-rescore applies corrected BUY+SELL pair metric to all 809 T2 wallets (CB-WATCHLIST-STALE fix)
- [ ] CB-DEDUP-REPEAT open (MED priority) — watch for 0xafbacaeeda-style repeats

### KrajekBot
- RETIRED 2026-04-19. See /root/PM_Krajek/RETIRED.md and FALSIFIED.md.

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
- [ ] HUB-ORPHAN-AUDIT — investigate /opt/hub/ files for orphaned duplicates of live files. /opt/hub/healthcheck.py confirmed orphan of /root/healthcheck.py. Trigger: next /opt/hub/ edit.
- [ ] DOC-DRIFT-REMEDIATION — 4-count finding Apr 15-20. Candidates: systemd-level alerting, evening_updater extension, WORKSPACE freshness check in morning-check.
- [ ] Polymarket exchange migration monitoring (@PolymarketDevs) — CTF allowance re-approval needed post-migration on all bot wallets
- [ ] SX Bet integration (CopyBot) — `sx_paper.jsonl` logging wired; review ~monthly

## Recent decisions (last 7 days)

- 2026-04-20: polybot.service killed (unauthorized 9-day paper-mode shadow); PolyArb dead-refs cleaned from 3 scripts (commit 654711c)
- 2026-04-20: CopyBot 4 bleeders demoted via CB-DECAY-MONITOR gate (commit e6dbce3); cohort reframed as bi-modal not broad decay
- 2026-04-17: CopyBot switched to 100% paper mode via systemd drop-in; 306 settled trades confirm wallet-selection cull was wrong
- 2026-04-16: CB-NOISE-PAPER-BUG fixed (commit e60221f) — `bet_size = min(PAPER_BET_SIZE_USD, bet_size)`
- 2026-04-15: CB-SIZING-FIX — 148/157 T2 wallets raised from stale $0.50 to $1.00 MIN_LIVE_BET_USD

## Blocked / waiting

- NewsBot gate: blocked on Polymarket resolution lag (0/121 settled, Apr 30 cohort is first realistic window)
- WeatherBot WB-RESUME: blocked until 14-day WB-28 verification completes (~Apr 22)

## Session conventions

- Morning: read this file first, then run morning check skill
- After any significant action: update "Open tasks" and "Recent decisions" here
- Evening: append to `/root/.agent/memory/episodic/session_log.jsonl`, commit
