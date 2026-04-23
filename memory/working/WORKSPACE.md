# Workspace — Live Task State

Last updated: 2026-04-23 (Thursday)
Updated by: Claude Chat (brain-sync recovery)

## Current focus

**Brain-sync recovery + strategic findings.** 2026-04-23 session surfaced major process gap: Claude Chat had been updating /root/docs/AllBots_TODO_CURRENT.md (deprecated handover layer) instead of WORKSPACE.md (authoritative brain) for an unknown duration. All sessions this week operated on week-old /mnt/project/ snapshots. Architecture diagram + brain-mirror verification (brain commit d5d3257) confirmed the correct flow. This session mirrors today's 6 findings into the brain and fixes the onboarding doc. Next session: evaluate whether to formally retire AllBots_TODO_CURRENT.md and /mnt/project/ as data sources, or keep parallel-update as a safety net. Also pending: CB-FLIP-GATE-TIMEOUT validates at Fri 2026-04-24 02:30 UTC.

## Open tasks by bot

### CopyBot
- [ ] CB-FLIP-GATE Path B data-source repair — Gamma unusable for 89.1% of historical cids. Candidates: CLOB resolution endpoint, bulk check_pm_settlement (activity_monitor.py:1354), paginated Gamma ?closed=true index build. Blocks timer enable.
- [ ] CB-FLIP-GATE backfill review — survival 0.20→25 / 0.25→29 / 0.30→33 / 0.35→39 / 0.40→45 of 129. Scale question: is a 33-45 wallet watchlist enough signal volume for the paper track?
- [ ] 0x2c1bf2a1 demote revisit — 21.1% flip, passes Path A and flip filter at ≥0.25. Yesterday's WR-based demote may have been collateral damage to a thesis-compliant wallet.
- [ ] Enable /tmp/copybot-flip-gate-shadow.timer — ONLY after Path B repair. Needs `systemctl daemon-reload` + `enable --now`.
- [ ] CB-WATCHLIST-DRIFT-AUDIT — investigate 2257→162 prune Apr 15-17, commit skip cause. Trigger: before next watchlist write.
- [ ] CB-MARTINGALE-WATCH-0x8a81855d — monitor for n≥50 or single-loss >$5. Current: 77.3% WR n=22 +$1.80 paper, on-chain flag -$32K Martingale.
- [ ] CB-DECAY-MONITOR re-eval — ~Apr 25 with fresh post-demote cohort.
- [ ] Sunday 06:00 UTC auto-rescore (SCAN-7) — Apr 26 is the next fire.
- [ ] CB-DEDUP-REPEAT open (MED priority) — watch for 0xafbacaeeda-style repeats.

**Added 2026-04-23:**
- [ ] CB-FLIP-GATE-TIMEOUT (ACTIVE) — shadow-timer TimeoutStartSec raised 3600→21600. Gate: Fri 2026-04-24 02:30 UTC complete run.
- [ ] CB-FLIP-GATE-TIER3 (GATED on CB-FLIP-GATE-TIMEOUT) — Tier-1 walk-forward null (0/960). Tier-3 is the real test. If Tier-3 also nulls: retire CB-FLIP-GATE apparatus.
- [ ] CB-APR14-AUDIT140-FINDING (GATED on CB-WATCHLIST-DRIFT-AUDIT + CB-WR-ZERO-ANOMALY) — 140/140 of Apr 14 cull Task 3 audit-driven demotes are CULL_FALSE_POSITIVE. Aggregate $7.34M profiler_net_pnl falsely demoted. Report at /opt/copybot/logs/cb_apr14_audit140_report_20260423.md.
- [ ] CB-WR-ZERO-ANOMALY (ACTIVE, MED) — 0x32ccd901 + 0x230287e2 show WR=0.000 with positive profiler_pnl over 100+ trades. Compute_profile edge case vs market-maker pattern vs data corruption.
- [ ] CB-BUYHOLD-MONITOR (MONITORING, LOW) — Residual 0xdbdd45 + 0xeca1e9 from Apr 8 postmortem. Gated on CB-HARVESTER-BUYHOLD.
- [ ] CB-HARVESTER-BUYHOLD (ACTIVE, MED) — Extend harvester to attribute P&L / band-rate to buy-only (BUY_HOLD) wallets. Unblocks CB-BUYHOLD-MONITOR and protects future culls from the blind spot.

### Shelved CopyBot tracks (closed this session)
- CB-EXIT-MIRROR — SHELVED by LATENCY audit: architecture permits (83.8% reachable) but design is deliberate BUY-only; the $61 gain came mostly from thesis-violator wallets that should be pruned at source, not mirrored downstream. Any revive requires lifting activity_monitor.py:1012 side!=BUY skip + adding SELL path to clob_executor.py (additive, not rewrite).
- CB-SCORE-RESOLVED-ONLY — SHELVED by FALLBACK audit: compute_metrics fallback at rescore_watchlist.py:142-159 has 0 fires in 30 days of logs + the Apr 14 full-T2 emergency pass. Latent defect; fix opportunistically on next rescore touch.

### KrajekBot
- RETIRED 2026-04-19. See /root/PM_Krajek/RETIRED.md and FALSIFIED.md.

### NewsBot
- [ ] May 7 extended go/no-go gate (was Apr 22 — extended due to 0 settled trades)
- [ ] Fuzzy dedup fix (9.8% duplicate leak)
- [ ] 30s burst debounce (48% calls <10s apart)
- [ ] `datetime.utcnow()` deprecation fix in `pipeline.py:78`

### WeatherBot
- [ ] Apr 22 WB-RESUME go/no-go — tomorrow's first gate
- [ ] Apr 22 WB-29 city divergence prior gate — tomorrow's second gate
- [ ] WB-30 (paper_settler fix) — first task next WeatherBot session
- [ ] SCT-AUTO-822 (9.0) — audit `/opt/weatherbot/noaa_scanner.py` against v3.8.0 `openapi.json`

### ScoutBot
- [ ] SCT-2 Reddit API approval still pending (submitted Mar 30)
- [ ] Periodic bulk retirement of PolyArb-tagged SCT-AUTO items (~weekly)

### Cross-cutting
- [ ] HUB-ORPHAN-AUDIT — /opt/hub/ files possibly orphaned. /opt/hub/healthcheck.py confirmed orphan.
- [ ] DOC-DRIFT-REMEDIATION — 4-count finding Apr 15-20. Candidates: systemd-level alerting, evening_updater extension, WORKSPACE freshness check in morning-check.
- [ ] Polymarket exchange migration monitoring (@PolymarketDevs) — CTF allowance re-approval needed post-migration on all bot wallets.
- [ ] Cost watch — 5 CC sessions on CopyBot today contributed to MTD $79.19 (528% over target). Tomorrow: single well-scoped CC session per bot, not sequential loops.

**Added 2026-04-23:**
- [x] INFRA-CHAT-ONBOARDING (DONE) — orientation doc at /root/.agent/CLAUDE_CHAT_ONBOARDING.md. Brain mirror base URL: https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/

## Recent decisions (last 7 days)

- 2026-04-23: Onboarding doc URL corrections — earlier Phase 2 report (brain commit d5d3257) added obfuscation suffix to repo URL based on a claude.ai project-share URL misread as the canonical GitHub path. Corrected to plain `molensloot77-collab/bot-brain` (public, unauthenticated, matches git remote). Separately: mirror fetch via web_fetch showed stale Apr 18 WORKSPACE content despite Apr 21 on-disk version and Apr 22 push — GitHub caching or push-reliability gap. Filed for follow-up investigation.
- 2026-04-23: Brain-rot diagnosed. Claude Chat had been updating AllBots_TODO_CURRENT.md (deprecated) instead of WORKSPACE.md (authoritative) for weeks. Brain-mirror read path verified via web_fetch — correct URL pattern documented in CLAUDE_CHAT_ONBOARDING.md.
- 2026-04-23: APR14-AUDIT-140 static scan found 140/140 CULL_FALSE_POSITIVE on Apr 14 cull Task 3 audit slice. $7.34M falsely demoted. Same BUY_HOLD bug-class as Apr 8, 7× the scope. Restore gated on CB-WATCHLIST-DRIFT-AUDIT re-vet path.
- 2026-04-21: CB-FLIP-GATE spec + observe-only shadow deployed (commits 2308c37, c1b6a36). Systemd timer staged at /tmp/ NOT enabled pending Path B data-source repair. Backfill on 129 T2 wallets complete. CB-EXIT-MIRROR and CB-SCORE-RESOLVED-ONLY shelved by audit findings.
- 2026-04-21: Hold-to-resolution thesis vs admission gate contradiction documented — `activity_monitor.py:6` docstring targets hold-to-resolution traders; `wallet_profiler_full.py:374-379` gate requires closed_trades≥30 (mechanically excludes them). 75.8% of T2 violates 20% tight-hold threshold.
- 2026-04-20: polybot.service killed (unauthorized 9-day paper-mode shadow); PolyArb dead-refs cleaned from 3 scripts (commit 654711c).
- 2026-04-20: CopyBot 4 bleeders demoted via CB-DECAY-MONITOR gate (commit e6dbce3). One (0x2c1bf2a1) queued for revisit under CB-FLIP-GATE threshold decision.
- 2026-04-17: CopyBot switched to 100% paper mode via systemd drop-in.
- 2026-04-16: CB-NOISE-PAPER-BUG fixed (commit e60221f).
- 2026-04-15: CB-SIZING-FIX — 148/157 T2 wallets raised from $0.50 to $1.00 MIN_LIVE_BET_USD.

## Blocked / waiting

- CB-FLIP-GATE timer enable: blocked on Path B data-source repair (Apr 22+)
- NewsBot gate: blocked on Polymarket resolution lag (0/121 settled, Apr 30 cohort is first realistic window)
- WeatherBot WB-RESUME: blocked until WB-28 verification completes (~Apr 22)

## Session conventions

- Morning: read this file first, then run morning check skill
- After any significant action: update "Open tasks" and "Recent decisions" here
- Evening: append to `/root/.agent/memory/episodic/session_log.jsonl`, commit
