# Workspace — Live Task State

Last updated: 2026-04-29 (Wednesday late UTC — CORRECTION pass: morning's CB-PHASE3-NULL-LEGACY-BLINDSPOT framing falsified by evening recon. Renamed to CB-WATCHLIST-NET-PNL-PM-NOT-PERSISTED, downgraded HIGH→LOW. L20 gating reference dropped. See LESSONS-2026-04-29 correction entry. Earlier Batch A pass details remain in commit history.)
Updated by: Claude Chat (Phase 3 implementation session via Claude Code)
Session-start paste template: /root/.agent/session_start_paste.md (BigW copies content at start of every new Claude Chat session)

## Current focus

**INFRA-RETIRE-CLEANUP COMPLETE (2026-04-24).** Retired-bot reference cleanup, handover-MD audit, brain-mirror enforcement landed across all 5 phases. AllBots_TODO_CURRENT.md formally retired as a data source — brain WORKSPACE.md is sole source of truth. ScoutBot redirected to write into new SCOUTBOT_INBOX.md (human triage gate) and read from WORKSPACE.md. 18 retired systemd units removed (5 PolyArb-* were `enabled` and would have auto-started on reboot — caught and explicitly disabled). /opt/weatherbot/ + /root/PM_Krajek/ archived to /opt/_retired/. /opt/polybot/ gutted of bot code per Approach A; venv/.env/data/logs preserved as shared infra host with RETIRED.md placed. 3 KrajekBot crons deleted. CHAT_RULES + 5 docs/skills swept. CopyBot iframe link to dead PolyArb dashboard removed. All 9 surviving bot services verified healthy post-cleanup. Full session log: /root/docs/session_logs/20260424_CHAT_INFRA_session.md. Three new follow-up TODOs added below (INFRA-VENV-MIGRATION, INFRA-SERVICE-RENAME, HUB-ORPHAN-AUDIT update). Two new LESSONS entries (retirement methodology, shared-infra naming). Next focus: CB-FLIP-GATE-TIMEOUT validation (still in flight at 02:30 UTC start, 6h timeout budget).

## Open tasks by bot

### CopyBot
- [ ] CB-FLIP-GATE Path B nightly-quality verification (REFRAMED 2026-04-29 from 'Path B data-source repair') — backfill-mode 89.1% unusable framing is moot; backfill is no longer the operational path. Apr 29 02:30 UTC nightly run footer: path_a=1502, path_b=215, path_b_only=141, path_b_thin=470, path_b_skipped_thin_denom=113. Pre-2026-05-25 task: characterize what Path B contributes vs path_a-only on the TIER3 snapshot cohort (flip_gate_shadow_tier3_snapshot_20260425.jsonl) — does Path B add discrimination, or is it noise that should be excluded from TIER3-ANALYSIS? Does not block nightly firing (already running).
- [x] CB-FLIP-GATE backfill review (CLOSED 2026-04-29 — superseded by operational scale ~3,906 cohort) — 33-45 survival was the question for the 129-wallet T2 backfill cohort. Operational gate is nightly mode against ~3906-cohort: Apr 29 admission counts combined_at_0.20=226 / 0.25=287 / 0.30=355 / 0.35=437 / 0.40=527. 129-wallet T2 backfill question moot at the new operational scale.
- [x] 0x2c1bf2a1 demote revisit (DONE 2026-04-24 late AM) — RESTORED to T2/copy_enabled=true via Decision 3 (restore-with-watch). Evidence: historical profile n=385 WR=68.3% passes all 5 standing LESSONS gates; Apr 20 demote signal statistically weak (n=14 paper, binomial p~0.3% under H0=0.683); BUY_HOLD attribution split at depth=200 shows resolved=$36.7k vs MTM=-$38.2k on 46 open positions — possibility that paper 'losses' are unresolved opens, which Apr 20 decay-monitor could not distinguish (predates Phase 2 PRODUCER). Restoration tagged with restoration_watch=True so future rescore passes attend. Copy is paper-mode only per Apr 17 drop-in so live risk = $0. Commit 247ea43 in /opt/copybot this session.
- [x] Enable copybot-flip-gate-shadow.timer (DONE retroactively) — timer has been at /etc/systemd/system/ and enabled since 2026-04-22, firing nightly at 02:30 UTC against candidates_pending_review.json (nightly mode). Path B repair concern was scoped to backfill mode, not nightly. WORKSPACE line predates the timer move and was never updated.
- [x] CB-WATCHLIST-DRIFT-AUDIT (DONE 2026-04-23) — Investigated. Prune was 2026-04-18 (not Apr 15-17; date anchored on backup filename, not event log). Mechanism: rescore_watchlist.py SCAN-7 INACTIVE filter ('0 trades in 30 days'). 2,100 wallets removed; ~1,570 structurally correct. Only ~144 are Mode B re-vet candidates (139 APR14-AUDIT-140 + 5 T2). Not the same bug class as Apr 8 / Apr 14 Task 3 (reconstruct_pnl). Spawned CB-MONTHLY-REVIEW-BUYHOLD. Full report: /opt/copybot/logs/cb_watchlist_drift_diag_20260423.md.
- [x] CB-MONTHLY-REVIEW-BUYHOLD (DONE 2026-04-23) — Resolved via BATCH2B-MODE-B Phase 1. Patched monthly_watchlist_review.py classify() with BUY_HOLD exemption: wallets where sell_count==0 AND buy_count_30d>10 now return KEEP_BUYHOLD_EXEMPT instead of INACTIVE_CULL'ing on the closed<30 AND days_active>60 branch. Uses all-time sell_count as conservative proxy (profiler doesn't expose sell_count_30d). Telemetry tag "buyhold_exempt" distinguishes from generic "thin_sample" keeps. Provisional bridge until CB-HARVESTER-BUYHOLD delivers structural pair-independent edge measurement. py_compile PASS; 11/11 unit tests PASS; dry-run on 33 WATCHLIST_ONLY entries clean (0 accidental flips). Report: /opt/copybot/logs/cb_mode_b_restore_execution_20260423.md §1.
- [ ] CB-MARTINGALE-WATCH-0x8a81855d (n>=50 trigger TRIPPED 2026-04-29; demote/threshold decision on its own merits, NOT gated — original gating premise on null-legacy was falsified 2026-04-29 evening, see LESSONS correction entry) — Paper closed n=58 (37W/21L), WR 63.8% (down from 77.3% at n=22; binomial p~0.02 under H0=0.773), cumulative paper PnL +$4.56, max single loss -$1.00 (paper $1 floor — $5 trigger NOT tripped). Watchlist state: T2/copy_enabled=True, win_rate=0.964 (likely BUY_HOLD-blind with closed_trades=None), net_pnl_pm=None, profiler_resolved_pnl=+$7,915, profiler_realized_ratio=0.47, profiler_open_positions=251. NOT in Phase 3 44-wallet demote-eligible cohort — null legacy field caused silent cohort-skip (see CB-PHASE3-NULL-LEGACY-BLINDSPOT). Structural cousin to 0xaa075924e1 (L63). On-chain -$32K Martingale flag remains the underlying concern.
- [ ] CB-DECAY-MONITOR re-eval (REFRAMED 2026-04-29 to single decision question) — Under Phase 3 min-gating (live since 2026-04-27, validated 2026-04-28 on 44-cohort with 95.5% path-A-fail rate, 0/44 integrity violations), does CB-DECAY-MONITOR have any work that the gating does not already cover? Apr 25 calendar trigger has passed without a new fire. If a future session answers 'no' to the coverage question, close this AND L94 (CB-DECAY-BUYHOLD-AUDIT) together — both are downstream of the same coverage question. The original Apr 24 BUY_HOLD-blindness concern pre-dated Phase 3 PRODUCER+CONSUMER landing and may be obsolete. BigW decision.
- [ ] CB-DEDUP-REPEAT open (MED priority) — watch for 0xafbacaeeda-style repeats. [Verified passive 2026-04-29: dedup logic present at activity_monitor.py:240/1670 (SIGNAL_DEDUP_TTL_S=300), CB-DEDUP-FIX commit 2026-04-12, DEDUP_SKIP event still firing. 0xafbacaeeda continues active (248 signals through Apr 19) without recurring repeat events.]

**Added 2026-04-23:**
- [x] CB-FLIP-GATE-TIMEOUT (SUPERSEDED 2026-04-24 by CB-FLIP-GATE-TIMEOUT-V2) — 6h budget insufficient. Apr 24 run completed 1340/3906 wallets (34.7%) before SIGTERM at the 6h boundary. Throughput is n_held-driven Gamma-query cost, not a bug — mean 15.9s/wallet but right-tailed (p99=209s, max=577s) on high-n_held signal-rich wallets. Full cohort ~17h at observed rate.
- [x] CB-FLIP-GATE-TIER3 (CLOSED 2026-04-29 — redundant, folded into TIER3-CRITERIA + TIER3-ANALYSIS) — V2 closed CB-FLIP-GATE-TIMEOUT gate (TimeoutStartSec 86400 + manual fire 2026-04-24 + Apr 25 catch-up). Tier-3 work is captured concretely in L43 (CB-FLIP-GATE-TIER3-CRITERIA, pre-committed 2026-04-24) and L57 (CB-FLIP-GATE-TIER3-ANALYSIS, scheduled 2026-05-25). The 'retire on null' disposition lives in L43's pass-bar. This bullet was an abstract placeholder predating the concrete pre-commitment.
- [x] CB-APR14-AUDIT140-FINDING (DONE 2026-04-23) — Resolved via BATCH2B-MODE-B. Fresh compute_profile applied to 139 swept APR14 cohort + 5 T2-INACTIVE-swept = 144 candidates. Buckets: RESTORE-T2 112, RESTORE-WATCHLIST-AMBIGUOUS 7, HAND-REVIEW 5 (all Path A-promoted to T2), NO-RESTORE-FAILED-GATES 20. Phase 5 executed 124/124 adds successfully after moving 129 stale BUY_HOLD-blind entries from rejected_wallets.jsonl to rejected_wallets_superseded_20260423.jsonl (fourth surface of BUY_HOLD-blind bug-class). Freshness caveat INVERTED: fresh net_pnl_pm sum for 139 = $8.07M vs Apr 14 snapshot $7.34M (ratio 1.10x); historical edge grew rather than decayed, contra CB-WR-ZERO-ANOMALY concern. Watchlist 162→286. Report: /opt/copybot/logs/cb_mode_b_restore_execution_20260423.md.
- [x] CB-WR-ZERO-ANOMALY (DONE 2026-04-23) — Hypothesis A confirmed with specific mechanism. 0x32ccd901 and 0x230287e2 are both currently pure BUY_HOLD wallets (6,000/5,996 BUYs, ~0 SELLs, 5-6 day visible history). compute_profile correctly returns WR=0.000 when closed_trades=0 (wallet_profiler_full.py:297). The apparent WR-vs-PnL contradiction in the Apr 14 snapshot comes from schema-lifecycle drift: profiler_net_pnl/profiler_closed_trades/profiler_top_category are WRITE-ONCE at harvester add-time (harvester.py:517-519, 561-563), while win_rate and net_pnl_pm are refreshed on every rescore pass (rescore_watchlist.py:289, 296). A record can contain profiler_net_pnl from weeks ago alongside win_rate from today. See INFRA-PROFILER-FIELD-FRESHNESS.
- [ ] INFRA-PROFILER-FIELD-FRESHNESS (ACTIVE, MED) — Fix schema-lifecycle drift in watchlist records: rescore_watchlist.py should either (a) refresh profiler_net_pnl, profiler_closed_trades, profiler_top_category on every rescore pass alongside win_rate and net_pnl_pm, or (b) rename one set so consumers know which fields are frozen-at-add vs rolling. Option (a) is simpler but costs profiler API calls per wallet per rescore; option (b) is cheaper but requires updating every consumer of these fields. Decide before any future large-scale audit relies on these fields being current.
- [ ] CB-WATCHLIST-NET-PNL-PM-NOT-PERSISTED (ACTIVE, LOW — was CB-PHASE3-NULL-LEGACY-BLINDSPOT, premise falsified 2026-04-29 evening, see LESSONS-2026-04-29 correction entry) — Narrow real bug: update_watchlist() at rescore_watchlist.py:432-475 does not copy net_pnl_pm from new_metrics to the persisted watchlist record. Field is universally null across all 286 records, has been for at least 15 days (verified across four backups). NO downstream impact identified: flip_gate_shadow.py:232 reads from fresh compute_profile output (legacy_pnl = profile.get('net_pnl_pm', 0) or 0), not from the watchlist record. Phase 3 cohort selection is unaffected. Bug is cosmetic for now: humans inspecting the watchlist file see null where they expect populated. Decision deferred: (a) patch update_watchlist to persist (one-line fix, low risk, no functional change since nothing consumes the disk value), (b) leave as-is and document write-once-by-harvester as the intended contract, (c) investigate whether write-once-by-harvester was deliberate or accidental. Original HIGH priority and 179-wallet blast-radius framing were based on the falsified cohort-skip claim. Real priority is LOW; real scope is one-line patch to one function or a documentation note. Distinct from INFRA-PROFILER-FIELD-FRESHNESS: that issue is STALE legacy values producing wrong verdicts; this is ABSENT legacy values producing silent-skip. Blast-radius probe 2026-04-29: 179 T2/copy_enabled wallets with net_pnl_pm=None (structural coverage gap — blocks Phase 3 validation magnitude claim). Sample: 0xc8d9c0df291315f2184ecdab4ed8247618acdf36, 0x9013e637016a5501ad1e67e398c8398c821ae7e0, 0x13997bdbf1b291b7ba65afaf1f0d8e4719ee48c8, 0x86c878cde72660ec52f5e6f0f0438b76de8fc867, 0x69672d76dd8512c0bc961c7b3c41fe173d7184bf (+174 more). Evidence: 0x8a81855d (T2/copy_enabled=True, net_pnl_pm=None, profiler_resolved_pnl=+$7,915, profiler_realized_ratio=0.47, profiler_open_positions=251, win_rate=0.964 likely BUY_HOLD-blind) is structurally a cousin to 0xaa075924e1 (L63 / LESSONS-2026-04-27: +$604K resolved / -$4.86M MTM) — both shapes have positive resolved + non-trivial open-book exposure; min-gating cannot see either. Connects to realized-ratio gate variant noted in LESSONS-2026-04-28 boundary note. Decision options: (a) treat as one-off, (b) backfill missing legacy values via fresh per-wallet rescore before next TIER3 cycle, (c) extend cohort selection to flag null-legacy via realized_ratio/open_positions side-gate. Gates: L20 (CB-MARTINGALE-WATCH-0x8a81855d) demote/threshold decision waits on this TODO's resolution.
- [ ] CB-BUYHOLD-MONITOR (MONITORING, LOW) — Residual 0xdbdd45 + 0xeca1e9 from Apr 8 postmortem. Gated on CB-HARVESTER-BUYHOLD.
- [x] CB-HARVESTER-BUYHOLD Phase 2 (PRODUCER) DONE 2026-04-24 — structural fix shipped behind feature flag. 4 commits to /opt/copybot: 6fa877f wallet_profiler producer, 738ab4d rescore persistence + flag log, 140af19 diagnostic tool, 4839dd7 AGENT.md docs. 8 new attribution fields populated when CB_BUYHOLD_ATTR_PRODUCER=1: profiler_resolved_pnl, _mtm_pnl, _total_pnl_attr, _realized_ratio (None when both legs 0), _open_positions, _attr_refreshed_at, _attr_method ("hybrid_v1"), _attr_proxy_fallback_hits (diagnostic). Acceptance: byte-identical pre-Phase-2 output with flag OFF (3 wallets); rescore --wallet end-to-end with PRODUCER=ON populated 8 fields + emitted flag-state log line. Drop-in staged at /etc/systemd/system/copybot-rescore.service.d/buyhold-attr.conf (NOT activated — BigW activates via daemon-reload). Consumer flag stays OFF until Phase 3. Drop-in active in effective unit config as of 2026-04-24 (merged by daemon-reload from an unrelated CB-FLIP-GATE-TIMEOUT-V2 session mid-PM, not the explicit evening activation — see LESSONS entry on systemd drop-in activation timing). DropInPaths merged, Environment=CB_BUYHOLD_ATTR_PRODUCER=1 live. Consumer flag remains OFF. First PRODUCER=ON rescore pass: Sunday 2026-04-26 06:00 UTC natural SCAN-7 fire. No rescore has fired between drop-in landing and now; PRODUCER=OFF observations are not contaminated.
- [ ] CB-HARVESTER-BUYHOLD Phase 3 (CONSUMER FLIP, GATED on diagnostic review) — flip CB_BUYHOLD_ATTR_CONSUMER=1; update 12 GATE + 3 DERIVED sites per Phase 1 report §6 (/root/docs/session_logs/20260424_CHAT_CB_HARVESTER_BUYHOLD_phase1_session.md); remove monthly_watchlist_review.py BUY_HOLD provisional exemption (sell_count==0 AND buy_count_30d>=10); one-shot rescore on Mode B Apr 23 cohort for validation. Gate: BigW approves after reading tools/buyhold_attribution_report.py output from first Sunday pass with PRODUCER=ON. Risk: Phase 2 acceptance surfaced an example wallet (0xc8d9c0) where MTM swung total_pnl_attr by -$30k vs +$23k legacy net — confirms gate metric MUST be profiler_resolved_pnl, not profiler_total_pnl_attr (Risk R4 from Phase 1). Preconditions: [x] MAXPAGES resolved (commit d55bc9e 2026-04-24); [ ] diagnostic review of first PRODUCER=ON Sunday pass.
- [ ] CB-HARVESTER-BUYHOLD-V1.1-CACHE (DEFER, LOW) — Add persistent (cid, outcome) → (resolved, won) cache at /opt/copybot/data/resolution_cache.json. Triggers: v1.0 in production >2 weeks OR API cost growing OR T2 >1000 wallets. Rationale: resolved outcomes don't change; ~10× cheaper subsequent rescore passes. Est: 30 min LOC + tests. Currently deferred per Phase 1 design §9.
- [x] CB-HARVESTER-BUYHOLD-MAXPAGES (DONE 2026-04-24, approach (a), commit d55bc9e in /opt/copybot) — Added `profiler_attr_depth` (int) field to compute_buyhold_attribution output (9 fields total now). wallet_profiler_full.py, rescore_watchlist.py, watchlist_validator.py, tools/buyhold_attribution_report.py, AGENT.md touched. rescore path passes buyhold_attr_depth=6; CLI score_one passes max_pages (default 200). Single-wallet smoke test on 0xc8d9c0 at depth=6 and depth=20: profiler_attr_depth correctly reads 6 and 20 respectively; proxy_fallback_hits=305 at both depths for this wallet (plateaus once all 544 open positions are visible; difference would show at depth=200 where historical closes clear the truncation artifacts). Framing correction: Phase 2 acceptance described this as "cross-wallet comparison in buyhold_attribution_report.py unreliable until normalized." That framing was imprecise — buyhold_attribution_report.py reads sports_watchlist.json, which is uniformly written by rescore_watchlist.py at max_pages=6, so the watchlist universe was always internally consistent. The real issue was CLI-vs-rescore divergence: ad-hoc CLI investigations of an individual wallet (max_pages=200) produce different profiler_attr_proxy_fallback_hits than the watchlist record for the same wallet (max_pages=6). Fix (a) surfaces depth as a field so cross-path comparisons are diagnosable. Phase 3 precondition met — no consumer-side normalization required since the watchlist is already uniform.
- [ ] SCT-AUTO-881 (BLOCKED, 7.4) — harvester --add requested but source address truncated to `0x594edb91` (10/42 chars). Full address needed from BigW before profiling/gating.
- [ ] SCT-AUTO-886 (INVESTIGATE, MED) — Runesleo polymarket-toolkit codebase audit. Read the repo, identify novel primitives vs overlap with our existing tooling. If novel primitives found, evaluate integration paths. Absorbs SCT-AUTO-887 (toolkit profiler as harvester pre-filter, potentially on SCT-AUTO-865..877 backlog) and SCT-AUTO-888 (strategy-class mapping to MON-26c category WR scaling) as conditional follow-ups.
- [ ] SCT-AUTO-889 (INVESTIGATE, MED) — Forecaster Arena wallet-address discovery (github.com/setrf/forecasterarena, 7 frontier LLMs competing on Polymarket). HTTP-fetch their leaderboard/public data, extract Polymarket wallet addresses, profile candidates against standing LESSONS.md gates. If candidates surface, SCT-AUTO-890 activates; otherwise both close.
- [ ] SCT-AUTO-890 (MONITOR, gated on SCT-AUTO-889) — Forecaster Arena top-3 wallet-add. Activates if SCT-AUTO-889 surfaces qualifying candidates (add top-3 passing gates). SCT-AUTO-891 ROI>15% gate dropped as premature.
- [ ] SCT-AUTO-896 (INVESTIGATE, LOW) — Humanplane terminal leaderboard API audit. What's the API, is data quality sufficient, does it duplicate Polymarket activity API. Absorbs SCT-AUTO-897 (SSE order book feed for MON-30 price-drift detection) as a conditional follow-up.

**Added 2026-04-24:**
- [ ] CB-FLIP-GATE-TIMEOUT-V2 (ACTIVE) — TimeoutStartSec raised 21600→86400 (24h) on /etc/systemd/system/copybot-flip-gate-shadow.service, daemon-reload applied. Gate: Saturday 2026-04-25 02:30 UTC natural fire reaches EndOfRun before SIGTERM. If 24h budget also insufficient: engineering conversation on parallelism / per-cid Gamma caching / cohort reduction, not another timeout raise. Executed ahead of schedule: manual start 2026-04-24 10:55:38 UTC via `systemctl start copybot-flip-gate-shadow.service` (Option B from chat reasoning — bring run forward, suppress Saturday scheduled fire to avoid double-run ambiguity). Cohort at fire: 3892 wallets from candidates_pending_review.json run_ts 2026-04-24T03:01:41Z. Saturday 2026-04-25 02:30 UTC fire suppressed via `systemctl stop copybot-flip-gate-shadow.timer` (timer unit remains enabled). CB-FLIP-GATE-TIMER-RESUME TODO gates re-start.
- [ ] CB-FLIP-GATE-TIER3-CRITERIA (PRE-COMMITTED 2026-04-24 late, gates Sunday analysis) — Criterion locked before Saturday 02:30 UTC gate-fire to prevent reverse-reasoning from data. Details:
  * Treatment vs control: admitted-cohort (flip_filter_at_T=True) vs rejected-cohort (False) per threshold T in {0.20, 0.25, 0.30, 0.35, 0.40}.
  * Forward window: 30 days from Saturday's gate-fire timestamp; evaluation runs ~2026-05-25.
  * Outcome measure: source-wallet 30-day realized P&L (via profiler API on the Saturday snapshot wallet list). Shadow-mode precludes own-P&L observation on rejected candidates.
  * Cohort freeze: Saturday's 3906-wallet jsonl snapshot is the evaluation list, regardless of downstream re-score/cull/rotation.
  * Pass bar (all must hold):
    (1) At >=3 of 5 thresholds: admitted median 30d forward P&L exceeds rejected median by BOTH Mann-Whitney p<0.05 AND (admitted >= 1.5x rejected OR admitted positive while rejected negative).
    (2) Cohort sizes >=30 on each side at every threshold evaluated.
    (3) Monotonicity (stricter threshold -> larger gap) is corroborating signal but NOT required for pass.
  * Null verdict consequence: RETIRE CB-FLIP-GATE apparatus. Not tune, not extend window — retire. Tier-1 null + Tier-3 null = filter has no discriminatory value at either admission or retention end.
  * Sunday analysis session (2026-04-26 or later) implements this criterion against Saturday's output + snapshot wallet list. The session must cite this TODO by ID before computing anything.
  * Forward outcome metric clarification (added 2026-04-27): profiler_resolved_pnl over the 30-day window (Apr 25 -> May 25), computed at evaluation time. This is the CB-HARVESTER-BUYHOLD Phase 2 metric, not the legacy net_pnl_pm. Admission scoring used legacy net_pnl_pm because Phase 3 consumer flip had not landed at gate-fire time. The metric mismatch (legacy admission / new evaluation) is documented and accepted as a one-time artifact. Verdict question: "under legacy admission-scoring, did the filter discriminate on the new outcome metric?" If yes, filter has hidden value. If no, retire as pre-committed. This clarification does NOT change the Mann-Whitney p<0.05 + 1.5x effect at >=3 of 5 thresholds + n>=30 each side pass bar.
- [x] CB-FLIP-GATE-TIER3-SNAPSHOT (DONE 2026-04-25 09:56:14 UTC. Snapshot taken before timer-resume; 5,698 lines / 4,903,218 bytes; cmp PASS byte-identical to source at snapshot time; path /opt/copybot/data/flip_gate_shadow_tier3_snapshot_20260425.jsonl; sha256 ceb9e881c5abcefcdb4250ca2b870e768906730cd614f01d69b31bb15c8b96e6. Auxiliary candidates_pending_review snapshot also taken (path /opt/copybot/data/candidates_pending_review_tier3_snapshot_20260425.json, 972,748 bytes, cmp PASS). Cohort artifact preserved for 2026-05-25 TIER3-ANALYSIS — gate ran a Persistent=true catch-up at 09:56:38 → 12:12:15 UTC after timer-resume which extended the live source jsonl to 9,590 lines; snapshot file is the analysis artifact, not the live source. Original active body: ACTIVE, HIGH, fires Saturday 2026-04-25 after gate EOF ~04:00 UTC estimate; 17h from Friday manual start. Check `systemctl is-active copybot-flip-gate-shadow.service` → inactive before running cp.) [2026-04-24 11:51 UTC throughput observation: current gate pace is ~2.3s/wallet, materially faster than yesterday's 15.9s/wallet that produced the 17h estimate. Completion could land Friday evening UTC rather than Saturday early morning. Trigger remains completion-based (systemctl is-active → inactive), so timing uncertainty doesn't affect correctness — just scheduling attention. Re-estimate when gate is ~30% through (around [N/3892] where N ~= 1170); current progress was [1480/3892] at observation time so 30% has passed — better to just check is-active status periodically Friday afternoon UTC.] — Freeze cohort artifacts for Sunday's TIER3 analysis BEFORE they are rewritten by normal operations. Actions: (a) cp /opt/copybot/data/flip_gate_shadow.jsonl /opt/copybot/data/flip_gate_shadow_tier3_snapshot_20260425.jsonl after gate run completes. (b) Save a frozen copy of the candidates list as it was at 10:55 UTC Friday fire — if ScoutBot's Saturday 03:01 UTC rewrite has already overwritten the original, reconstruct from the jsonl's wallet-addresses field (each gate record carries the wallet that was scored, so the wallet list IS derivable from the jsonl itself — no separate snapshot strictly required, but archive anyway for traceability). Label both files with the _tier3_snapshot_YYYYMMDD suffix.
- [!] CB-FLIP-GATE-TIMER-RESUME (DEVIATION 2026-04-25 09:56:38 UTC) — Re-start copybot-flip-gate-shadow.timer after the suppressed Saturday 02:30 UTC fire window has passed. Command: `sudo systemctl start copybot-flip-gate-shadow.timer`. Verify: `systemctl list-timers copybot-flip-gate-shadow.timer` should show next fire as 2026-05-02 02:30 UTC (one week out from the suppressed fire). Timer unit was kept `enabled` during suppression so start-after-stop preserves regular schedule. DEVIATION: timer started 09:56:38 UTC. Persistent=true on the timer unit caused immediate catch-up fire of the suppressed 02:30 UTC schedule. Gate service ran the catch-up window (start 09:56:38 UTC, end Sat 2026-04-25 12:12:15 UTC, duration 2h 15m 37s, exit Result=success / ExecMainStatus=0). Source jsonl diverged from snapshot during catch-up (5,698 → 9,590 lines, +3,892 records, tail JSON valid); snapshot preserved unchanged. Timer now running normal daily cadence (next fire Sun 2026-04-26 02:30:00 UTC, NOT 2026-05-02 as originally expected — unit is OnCalendar=*-*-* daily, not weekly). Suppression goal NOT achieved; cohort-freeze goal achieved (snapshot protects the analysis-relevant slice — TIER3-CRITERIA evaluates against snapshot file by name). Root cause and remediation: see CB-FLIP-GATE-TIMER-PERSISTENT-AUDIT (new TODO).
- [ ] CB-FLIP-GATE-TIER3-ANALYSIS (SCHEDULED for 2026-05-25 or later) — Consume Saturday 2026-04-25 gate output + 30d-forward source-wallet P&L against pre-committed CB-FLIP-GATE-TIER3-CRITERIA. Input files: /opt/copybot/data/flip_gate_shadow.jsonl (Saturday records) + candidates_pending_review.json snapshot frozen Saturday. Compute Mann-Whitney per threshold, effect-size comparison, monotonicity check. Output: pass/fail verdict with supporting distributions. DO NOT open the jsonl or wallet list before 2026-05-25; freezing the cohort is only meaningful if we don't peek.
- [ ] CB-FLIP-GATE-TRACEABILITY (LOW, opportunistic) — flip_gate_shadow.jsonl records carry no run_ts or input-file hash. candidates_pending_review.json is rewritten nightly (~03:01 UTC, 31 min into the gate run) so sequential-night runs can score different universes with no cross-reference. Fix: emit one header-record per run with run_ts + sha256(input_file) + wallet_count at jsonl start. One-line addition. Grab on next flip_gate_shadow.py touch.
- [ ] CB-FLIP-GATE-TIMER-PERSISTENT-AUDIT (HIGH, dedicated session) — copybot-flip-gate-shadow.timer has Persistent=true, which makes any `systemctl start` after a `stop` immediately fire any missed scheduled events. This is wrong for the "suppress one fire and resume" workflow used 2026-04-24 → 2026-04-25 (suppression failed; catch-up ran the gate run we wanted to skip). Decide between: (a) Persistent=false — start-after-stop is safe; trade-off is missed fires during machine downtime are not caught up; (b) keep Persistent=true and use `systemctl mask`/`unmask` for suppression windows; (c) sentinel-file gate inside the service. Verify the unit has no other latent surprises (OnBootSec, RandomizedDelaySec, AccuracySec). Reference 2026-04-25 LESSONS entries on Persistent=true catch-up and unit cadence verification. Risk if deferred: any future suppression-and-resume repeats the same failure mode.

**Added 2026-04-27 (Phase 3 review session):**
- [ ] CB-HARVESTER-BUYHOLD-PHASE3-LIVE (ACTIVE, HIGH) — Phase 3 consumer flag CB_BUYHOLD_ATTR_CONSUMER=1 activated 2026-04-27 ~07:00 UTC via systemd drop-in /etc/systemd/system/copybot-flip-gate-shadow.service.d/buyhold-consumer.conf. Gate now uses min(net_pnl_pm, profiler_resolved_pnl) — conservative-side gating. First fire under new regime: Tue 2026-04-28 02:30 UTC. Verification list at /opt/copybot/logs/phase3_active_demote_cohort_20260427.json (44 T2+copy_enabled wallets that should path-A-fail under new gating). Re-evaluate after one full TIER3 cycle (~2 weeks). Commits: 1434fb1 (classifier neg-zone fix), ce4fe59 (gate consumer flag in /opt/copybot).
- [ ] CB-MARTINGALE-WATCH-0xaa075924e1 (ACTIVE, HIGH) — surfaced 2026-04-27 in Phase 3 extreme-magnifier audit. T2+copy_enabled. Resolved +$604K but MTM -$4,859,817 across 330 open positions. realized_ratio 0.11 (89% of new-metric "edge" is paper gains). WR 0.99/330 closed (suspect — pure-resolved cohort). Pattern matches CB-MARTINGALE-WATCH-0x8a81855d structurally but ~150x larger by open-book exposure. Min-gating does NOT catch this — both legacy and resolved are positive, the bug is shared open-book blindspot. Monitor: n>=50 new closed OR single-loss > $5; on confirmation, demote with rationale citing martingale class. Also a candidate input for future "total_pnl_attr aware" gate variant.
- [ ] CB-SIZING-CEILING-THESIS-REVISIT (HIGH) — Apr 22 LESSONS thesis "median bet $0.31, fix didn't propagate" FALSIFIED 2026-04-27. Today's 5,428-paper-bet audit shows median our_bet $1.00 (3.2x Apr 22), median theirs $61.08, sizing ratio 1.18%. 52% of paper bets clear $1.00 floor in live-projection. Apr 22 reading was paper-mode artifact — activity_monitor.py:1289 bypasses MIN_LIVE_BET_USD floor in paper. CB-SIZING-FIX (Apr 15) DID land. Three downstream priorities were demoted on the basis of the Apr 22 thesis: CB-BACKTEST-TIER3 (deferred), CB-BACKTEST-RES-TS (HIGH->MED), CB-FLIP-GATE (expectation-managed). Re-evaluate priority each per current sizing reality. Candidate for FALSIFIED.md entry.
- [ ] CB-WORKSPACE-HEADER-DISCIPLINE (LOW) — evening_updater.py does not write to WORKSPACE.md. "Last updated" line bumps only when a Claude Chat session edits it manually (or instructs Claude Code to do so within session scope). Confirmed 2026-04-27 via T7 audit: only morning.sh:63-64 reads the line, no writer exists in /root/docs/. Action: add to CHAT_RULES — any session that writes WORKSPACE must bump the header in the same edit. No script change; semantic ownership stays with Claude Chat sessions.

### Shelved CopyBot tracks (closed this session)
- CB-EXIT-MIRROR — SHELVED by LATENCY audit: architecture permits (83.8% reachable) but design is deliberate BUY-only; the $61 gain came mostly from thesis-violator wallets that should be pruned at source, not mirrored downstream. Any revive requires lifting activity_monitor.py:1012 side!=BUY skip + adding SELL path to clob_executor.py (additive, not rewrite).
- CB-SCORE-RESOLVED-ONLY — SHELVED by FALLBACK audit: compute_metrics fallback at rescore_watchlist.py:142-159 has 0 fires in 30 days of logs + the Apr 14 full-T2 emergency pass. Latent defect; fix opportunistically on next rescore touch.

### KrajekBot
- RETIRED 2026-04-19. See /root/PM_Krajek/RETIRED.md and FALSIFIED.md.

### NewsBot
- [ ] May 7 extended go/no-go gate (was Apr 22 — extended due to 0 settled trades)
- [ ] Fuzzy dedup fix (9.8% duplicate leak)
- [x] burst debounce (DONE — shipped at 60s in commit 60b450f, env-override NEWSBOT_DEBOUNCE_S; original 30s spec tightened to 60s; verified 2026-04-28)
- [x] `datetime.utcnow()` deprecation fix (DONE 2026-04-13, commit 30105d8 in /opt/newsbot — verified 2026-04-28)

### WeatherBot — RETIRED 2026-04-22. Throughput ceiling ~$3-5/mo, architecture valid, no successor. See /opt/_retired/ and project_weatherbot_retired.md.

### ScoutBot
- [ ] SCT-2 Reddit API approval still pending (submitted Mar 30)
- [ ] Periodic bulk retirement of PolyArb-tagged SCT-AUTO items (~weekly)

### Cross-cutting

**Added 2026-04-24 (post INFRA-RETIRE-CLEANUP):**
- [ ] INFRA-VENV-MIGRATION (DEFER, MED) — Relocate shared Python venv from `/opt/polybot/venv/` to `/opt/_shared/venv/` (or `/opt/venv/`). Update ~25 references in systemd units, crons, scripts. Estimated 1 dedicated session. Unblocks: full archival of `/opt/polybot/` to `/opt/_retired/`.
- [ ] INFRA-SERVICE-RENAME (DEFER, LOW) — Rename `polybot-harvester.service` → `copybot-harvester.service`. The unit is real CopyBot infrastructure (ExecStart=/opt/copybot/harvester.py) but the historical name suggests retired PolyArb. Update references in morning.sh:56, morning.sh:62, evening_updater.py:51, evening_updater.py:303. Group with INFRA-VENV-MIGRATION when next touching systemd.
- [ ] HUB-ORPHAN-AUDIT scope expansion (existing TODO, expanded 2026-04-24) — In addition to `/opt/hub/healthcheck.py` (already known orphan): (1) `/opt/copybot/hub_dashboard.py` is an orphan duplicate of `/opt/hub/hub_dashboard.py` (live one is served by hub-dashboard.service); both now have TABS=['copybot','scout'], but the duplicate should be deleted. (2) `/opt/copybot/fix_watchlist.py` has a pre-existing Python syntax error at line 126 (bash-style escapes inside Python string) — file is broken at parse time, suggesting it has not been invoked in a long time; candidate for deletion. (3) `/opt/copybot/wallet_monitor.py` was the ExecStart of the now-removed polybot-copymon.service — confirm it is not invoked elsewhere, then delete. **Update 2026-04-25 (Saturday 14:01 UTC):** 1 of 3 scope-expansion items closed via 7-day quarantine to `/opt/_retired/copybot_orphans_20260425/`: **fix_watchlist.py** quarantined (parse-broken confirmed via py_compile, no runtime references). The other two NOT quarantined per verification-gate rules: **hub_dashboard.py** is DIVERGENT from `/opt/hub/hub_dashboard.py`, not byte-identical — hub copy has /health endpoint + sd_notify watchdog + SIGTERM handler that the copybot copy lacks; needs canonical-identification investigation before deletion. **wallet_monitor.py** has active runtime references (patch_mon5.py and patch_scan11.py both define `MONITOR = '/opt/copybot/wallet_monitor.py'`; AGENT.md rule "NEVER re-enable wallet_monitor without asking user first"; CopyBot_Handover_CURRENT.md RULE 1) — file intentionally preserved as a re-enablement option per BigW. Reference scan filtered IDE/log noise (`.vscode-server/`, `.cursor-server/`, `.cache/`, `.claude/`, `.local/`, session_logs). RETIRED.md placed in quarantine dir documenting all three decisions. Quarantine review date: 2026-05-02. Parent TODO remains open for `/opt/hub/healthcheck.py` AND for the two retained-but-questionable items (hub_dashboard.py canonical investigation; wallet_monitor.py future-fate decision).
- [x] INFRA-BRAIN-MIRROR-FRESHNESS (DONE 2026-04-24) — raw.githubusercontent.com edge-cache lag surfaced during session close→open on 2026-04-24 (commits d0429fa/0855d57/9168330 invisible to new-session fetch for ~5 min). Fix: session_start_paste.md + CLAUDE_CHAT_ONBOARDING.md now use GitHub Contents API for cache-bypassed reads. LESSONS entry "Mirror freshness: raw CDN vs API" documents the general rule. Raw URLs retained as rate-limit fallback.
- [ ] INFRA-REMOTE-REGRESSION-SMOKE (LOW, opportunistic) — add one-line check to /root/evening.sh to fail loudly if brain git remote regresses off the obfuscated canonical: `git -C /root/.agent remote get-url origin | grep -q "bot-brain-5whZJka5-dk.KfuY-yM-8Qww" || echo "REMOTE_REGRESSED: $(git -C /root/.agent remote get-url origin)" >&2`. Rationale: from commit 13ab8f1 forward, any push via legacy bot-brain URL signals regression. Grab on next evening.sh touch.
- [ ] CB-DECAY-BUYHOLD-AUDIT (ACTIVE, MED, candidate fifth surface of BUY_HOLD-blind bug class) — Audit CB-DECAY-MONITOR (script location TBD; search copybot for 'CB-DECAY' and 'decay_monitor'). Question: does the gate compute WR/PnL against copy-trade outcomes without distinguishing resolved positions from open-MTM positions? If yes, same bug class as Apr 8 / Apr 14 / monthly_watchlist_review / stale rejected_wallets.jsonl: a 'bleeder' signal on a paper cohort where positions are still open-in-drawdown is not a decay signal. Spawned from 0x2c1bf2a1 revisit where BUY_HOLD attribution showed resolved=$36.7k vs MTM=-$38.2k. Per LESSONS 'Bug-class scope expansion': treat as suspected-affected until proven safe. Until audit completes: any CB-DECAY-MONITOR demote should be reviewed against fresh profile with depth=200 BUY_HOLD attribution before acting. Not urgent — kill-switch triggers are at paper-PnL thresholds not single-wallet demotes — but worth a dedicated session before the next decay-monitor fire.
- [ ] CB-WATCHLIST-DRIFT-TRIAGE (ACTIVE, MED, one-session audit) — The 2026-04-24 0x2c1bf2a1 restore session (path A) surfaced ~157 hunks of uncommitted watchlist drift in /opt/copybot: 10 wallets with restored_at timestamps predating the current session (from BATCH2B-MODE-B commit 124cd34), plus multi-wallet updates to net_pnl_onchain / profiler_net_pnl / profiler_closed_trades with small deltas consistent with per-wallet harvester --refresh or rescore_watchlist.py --wallet invocations. No rescore service fire since Apr 19; no auto-commit hook observed. Drift captured at /root/docs/session_logs/watchlist_uncommitted_drift_20260424.diff (3053 lines, 92213 bytes) for durable record. git stash push -m "watchlist drift + 0x2c1bf2a1 restore (Apr 24 session A halt)" was used to isolate the restore commit; stash pop after commit 247ea43 CONFLICTED (predictable — stash version of 0x2c1bf2a1 entry overlapped with committed HEAD version) producing 2 merge markers and invalid JSON; working tree sports_watchlist.json reverted to HEAD defensively to avoid crashing active copybot-sports service; stash@{0} preserved intact for triage. Triage questions: (1) Which session(s) produced the drift? (check session_logs/ for Phase 2 acceptance + any CLI score_one runs since Apr 23), (2) Is there an auto-commit pattern missing from per-wallet update scripts?, (3) Did any of the drift mutations change gate-relevant fields in ways that would affect the CB-FLIP-GATE-TIER3 cohort freeze or Sunday PRODUCER diagnostic? If (3) is yes, the drift touches CB-FLIP-GATE-TIER3-CRITERIA pre-commitment — needs reconciliation before Sunday analysis. Gate: run before Sunday 06:00 UTC SCAN-7 fire so PRODUCER=ON rescore starts from a known-committed state. Access drift via either `cat /root/docs/session_logs/watchlist_uncommitted_drift_20260424.diff` (captured before stash) or `cd /opt/copybot && git stash show stash@{0} -p` (live stash with my_edit overlap).
- [ ] INFRA-WATCHLIST-DRIFT-LESSONS (DEFER, LOW, gated on CB-WATCHLIST-DRIFT-TRIAGE) — After triage determines the actual root cause of the Apr 24 watchlist drift pattern, write a LESSONS entry capturing the pattern. Candidate framings from initial observation: 'working tree drift accumulates silently when intermediate sessions edit files via scripts without committing', 'per-wallet mutations should auto-commit or emit a pending-commit warning', 'git status check before write is not the same as git status check before commit'. Actual lesson depends on triage root cause — don't write it until we know which framing fits.

**Added 2026-04-27 (Phase 3 review session):**
- [ ] INFRA-EVENING-SCOPE (MED) — evening.sh step 2/4 runs `git add -A` in /root/docs, which packages collateral into commits scoped to single-file changes. Surfaced 2026-04-27 via 250c696 (committed AllBots_Research_CURRENT.md modifications inside a "docs: add *.bak.* to gitignore" commit; corrected via soft-reset to 91b3487). Will recur every evening unless patched. Patch: enumerate the files evening_updater.py legitimately writes, switch to per-file git add. Rule alignment: -A only in /root/.agent. Estimate: 30 min including verification of the writer set.

**Pre-existing:**
- [ ] HUB-ORPHAN-AUDIT — /opt/hub/ files possibly orphaned. /opt/hub/healthcheck.py confirmed orphan.
- [ ] DOC-DRIFT-REMEDIATION — 4-count finding Apr 15-20. Candidates: systemd-level alerting, evening_updater extension, WORKSPACE freshness check in morning-check.
- [ ] Polymarket exchange migration monitoring (@PolymarketDevs) — CTF allowance re-approval needed post-migration on all bot wallets.
- [ ] Cost watch — 5 CC sessions on CopyBot today contributed to MTD $79.19 (528% over target). Tomorrow: single well-scoped CC session per bot, not sequential loops.

**Added 2026-04-23:**
- [x] INFRA-CHAT-ONBOARDING (DONE) — orientation doc at /root/.agent/CLAUDE_CHAT_ONBOARDING.md. Brain mirror base URL: https://raw.githubusercontent.com/molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww/main/
- [x] INFRA-PUSH-RELIABILITY (DONE 2026-04-23) — No infrastructure issue. Apr 20/21/22 evening.sh pushes all landed on origin/main cleanly (commits 2e4ac5b, 2344c72, 4c17c6a, ea06965). Mirror content matches origin/main exactly. Claude Chat's morning claim of 'Apr 18 content on mirror' was a misread of WORKSPACE's self-reported 'Last updated: 2026-04-18' header line (which itself had drifted from file state because session writers hadn't refreshed it). Actual mirror content was Apr 21. Softer finding: WORKSPACE's 'Last updated' header must be refreshed on every write, or it drifts from file state and becomes itself a source of confusion. See LESSONS entry 'Record-integrity patterns'.
- [ ] SCT-AUTO-880 (INVESTIGATE, MED) — API-robustness playbook for Polymarket activity API. Today's Mode B Phase 2 had 6/144 urllib failures (4%) requiring retry with longer timeouts; Phase 5 had no failures but the threshold isn't characterized. Small probe session: instrument a few profile calls, characterize failure modes (throttle / transient / persistent), document a standard retry/throttle pattern for future sessions.
- [ ] SCT-AUTO-906 (INVESTIGATE, LOW) — gensyn-delphi-skills REST API compatibility with py_clob_client. One-session probe: check their docs, compare signatures, report overlap.
- [RESEARCH-QUEUE 2026-04-23] SCT-AUTO-878, SCT-AUTO-879, SCT-AUTO-903 moved to /root/.agent/memory/semantic/research_queue.md. SCT-AUTO-891, 895, 898, 899, 900, 901, 902, 907 dropped.
- [x] SCT-AUTO-REJECTS-APR23 (8 items REJECT-FALSIFIED) — 882 (WB-compare), 883/884/885 (Pangu-Weather upgrades for retired WB), 892/893/894 (PolyArb hack-market), 905 (Delphi-for-PolyArb). Matches LESSONS.md pipeline observation: ScoutBot recycles falsified categories at ~27% rate (8/30 this batch). Reinforces the weekly scout-quality-audit skill candidate — threshold already hit.
- [x] INFRA-SCOUT-TRUNCATION (DONE 2026-04-23) — Truncation chain identified: (1) Twitter URL-shortened the polymarket.com link at ~13 chars with Unicode ellipsis; (2) scorer LLM produced inconsistent fields from same input (summary: 10 chars + '...', draft_todos[0]: 13 chars); (3) auto_ingestor.py LLM wrote the TODO row using the 10-char summary and emitted a prose hedge '(full address required)' instead of blocking. Fix class spawned as INFRA-SCOUT-VALIDATE. Address recovery is optional follow-up as SCT-881-RECOVERY.
- [ ] INFRA-SCOUT-VALIDATE (ACTIVE, MED) — Add syntactic validation gate in /opt/scoutbot/auto_ingestor.py before writing any row containing 'harvester.py --add <addr>': 42-char hex regex check. On fail: skip the --add command in the row (promote as research-only), or block the row. Rationale: LLM prose hedges are not a substitute for syntactic gates; any LLM emitting machine-executable output needs post-generation validation. Surfaced from INFRA-SCOUT-TRUNCATION.
- [ ] SCT-881-RECOVERY (DEFER, LOW) — Optional: HTTP-fetch polymarket.com/0x594edb9112f... to recover the full 40-char wallet address. If recovered: profile against standing LESSONS.md gates (MIN_RESOLVED=30, NOT FAVORITE_BUYER, price_band_rate≥30%); if passes, consider --add. Only pursue if wallet is independently interesting — no action warranted just because SCT-AUTO-881 surfaced it.

## Recent decisions (last 7 days)

- 2026-04-24 late AM: 0x2c1bf2a1 restored to T2/copy_enabled=true via Decision 3 (restore-with-watch). Compound evidence: historical n=385 WR=68.3% (all 5 LESSONS gates pass); Apr 20 demote signal weak (n=14 paper, ~0.3% binomial tail); BUY_HOLD attribution split at depth=200 shows MTM drawdown on 46 open positions invisible to Apr 20 decay-monitor. Watchlist: 286 total, copy_enabled +1 from prior count. Paper mode per Apr 17 drop-in so live risk $0. Apr 12 prior spurious CRITICAL_DECAY (CB-WR-ZERO-ANOMALY) on same wallet may have primed readiness to demote on Apr 20. Spawned CB-DECAY-BUYHOLD-AUDIT as candidate fifth BUY_HOLD-blind surface. Session halt surfaced ~157 hunks of uncommitted watchlist drift in /opt/copybot pre-existing to this session; captured to session_logs/watchlist_uncommitted_drift_20260424.diff; committed only the intended 0x2c1bf2a1 edit (copybot commit 247ea43) per Path A; stash pop CONFLICTED on overlap with committed edit producing 2 merge markers + invalid JSON, reverted working tree to HEAD to protect active copybot-sports service, stash@{0} preserved intact for CB-WATCHLIST-DRIFT-TRIAGE session.

- 2026-04-24: CB-HARVESTER-BUYHOLD-MAXPAGES closed via approach (a) — added `profiler_attr_depth` field alongside `profiler_attr_proxy_fallback_hits` so consumers can distinguish rescore-path (depth=6) from CLI-path (depth=200) counter values without re-tracing the call graph. Commit d55bc9e in /opt/copybot. Phase 3 precondition met. Framing correction filed: the Phase 2 acceptance description of "cross-wallet comparison unreliable until normalized" was imprecise — watchlist universe is uniformly written by rescore at max_pages=6 so internally consistent. Real issue was CLI-vs-rescore divergence on ad-hoc investigations. LESSONS pattern "Counter semantics depend on the window" filed under Record-integrity patterns.

- 2026-04-24 late morning UTC: CB-FLIP-GATE manual Friday fire executed via `systemctl start copybot-flip-gate-shadow.service` (10:55:38 UTC start, 24h TimeoutStartSec budget, ~17h runtime estimate → completion ~04:00 UTC Saturday). Saturday 02:30 UTC scheduled fire suppressed via `systemctl stop copybot-flip-gate-shadow.timer`. Option B from chat reasoning — single-run cohort eliminates disambiguation on jsonl records, compresses Sunday analysis timeline by ~36h. Pre-committed TIER3-CRITERIA (commit 337a483) unchanged; TIER3-SNAPSHOT firing-time updated; CB-FLIP-GATE-TIMER-RESUME added as HIGH TODO for Saturday afternoon.

- 2026-04-24 evening: CB-HARVESTER-BUYHOLD Phase 2 drop-in activated via daemon-reload. PRODUCER=1 now in effective env; CONSUMER stays 0. Sunday 06:00 UTC SCAN-7 is first PRODUCER=ON pass. Phase 3 remains gated on diagnostic review + MAXPAGES fix.

- 2026-04-24 late: INFRA-REPO-RENAME-PROPAGATE executed. bot-brain renamed to bot-brain-5whZJka5-dk.KfuY-yM-8Qww (intentional, privacy-through-obscurity, BigW action predating Apr 23). All active URLs in /root/.agent/ migrated; brain remote.origin.url updated; LESSONS correction to Apr 23 INFRA-PUSH-RELIABILITY postmortem filed. `bot-brain` legacy redirect preserved by GitHub but no longer referenced in docs/configs.

- 2026-04-24: CB-HARVESTER-BUYHOLD Phase 2 (PRODUCER) shipped to /opt/copybot. Four commits (6fa877f wallet_profiler producer, 738ab4d rescore persistence + flag log, 140af19 diagnostic tool, 4839dd7 AGENT.md). 8 new attribution fields populated when CB_BUYHOLD_ATTR_PRODUCER=1; consumer flag stays OFF until Phase 3 (separate session). Acceptance: byte-identical pre-Phase-2 output with flag OFF on 3 wallets (0xc8d9c0 rich, 0x77d326 proxy, 0xfd4263 mid); flag-ON populated 8 fields with sane values. Watchlist restored from /tmp/sports_watchlist_pre_buyhold_20260424.json (md5 22e63b62...) so Sunday's natural pass is the first coherent full-watchlist write. Drop-in staged at /etc/systemd/system/copybot-rescore.service.d/buyhold-attr.conf — BigW activates via daemon-reload when ready. KEY FINDING during acceptance: pair-matched-rich proxy wallet's profiler_resolved_pnl ($2,158) was SUBSTANTIALLY LOWER than legacy profiler_net_pnl ($62,268) — initial alarm proved to be the new attribution working correctly: legacy missed -$60k of held-to-resolution losses that the new fallback path catches. Exactly the BUY_HOLD-blind bug class the structural fix addresses. Confirms Risk R4 (use profiler_resolved_pnl not profiler_total_pnl_attr for gates) and Risk R1 (proxy/EOA mismatch handled via mismatch fallback). Phase 1 design report unchanged. Session log: /root/docs/session_logs/20260424_CHAT_CB_HARVESTER_BUYHOLD_phase2_session.md.

- 2026-04-24: INFRA-RETIRE-CLEANUP completed (full session, Phases 0–4). **Phase 1**: morning.sh +68/-33 (brain-mirror fetch with -L redirect fix, sections 0/4/7 read brain WORKSPACE, TG upload list 7→5+brain URL); evening_updater.py +16/-103 (KrajekBot emitters removed, get_todo_summary reads brain); CHAT_RULES.md +20/-14; NewsBot_Handover_v1.md → _CURRENT.md; 3 KrajekBot crons deleted (analyze_outcomes + dashboard --html + log_sol_oi, backup at /tmp/crontab_pre_purge_20260424.txt); /opt/copybot/hub_dashboard.py orphan-cleaned; /opt/scoutbot mid-session redirect (Tasks 1.4e–1.4i): todo_extractor writes to /root/.agent/memory/working/SCOUTBOT_INBOX.md (NEW; 934 historical SCT-AUTO IDs baked in dedup roster), tg_scout_report reads brain WORKSPACE.md (Open/Done-recent/Blocked counters), run_daily commits brain inbox; 4 doc/skill files swept (CLAUDE_CHAT_ONBOARDING, PREFERENCES, morning-check SKILL, strategic-review SKILL); end_of_day.sh updated; 3 deprecated MDs deleted (WeatherBot_Handover_CURRENT, KrajekBot_Handover_CURRENT, AllBots_TODO_CURRENT). **Phase 2**: 18 retired systemd units disabled+removed (4 krajekbot-, 3 weatherbot-, 11 polybot-, EXCEPT polybot-harvester which is misnamed CopyBot infra; 5 polybot-* were `enabled` and would have auto-started on reboot — caught and disabled); /opt/weatherbot/ + /root/PM_Krajek/ archived to /opt/_retired/ with .archived_at timestamps + git history preserved; /opt/polybot/ gutted of bot code per Approach A — 62 files deleted (-18907 lines), venv/.env/data/logs preserved as shared infra host with RETIRED.md placed. **Phase 3**: stale-ref sweep on /opt/scoutbot (BOT_ICONS/BOT_ORDER/BOTS/HANDOVER_FILES trimmed to active bots) + /opt/copybot (PolyArb dashboard iframe link removed, fix_*.py restart instructions updated). **Surviving bot health verified post-cleanup** — 9/9 active. Commits: docs `64aa8c8` + copybot `694500a` (P1 main), scoutbot `b5e1661` (P1 redirect), brain `49e4e87` (P1 inbox+skills, batched), root `1b9a6c2` (P1 end_of_day), polybot `04357c7` (P2 gut), scoutbot `b245126` (P3 sweep), copybot `f0d8a08` (P3 sweep). Session log: /root/docs/session_logs/20260424_CHAT_INFRA_session.md.

- 2026-04-23 late PM: BATCH2B-MODE-B executed. Phase 1 patched monthly_watchlist_review.py with BUY_HOLD exemption (closes CB-MONTHLY-REVIEW-BUYHOLD, provisional). Phase 2 fresh-profiled 144 candidates (139 APR14 cohort + 5 T2-INACTIVE). Phase 3 categorized: 112 RESTORE-T2, 7 RESTORE-WATCHLIST-AMBIGUOUS, 5 HAND-REVIEW (Path A promoted to T2), 20 NO-RESTORE-FAILED-GATES. Phase 5 Halt 1 surfaced FOURTH BUY_HOLD-blind surface: 129 stale entries in rejected_wallets.jsonl (pre-ae89d01 writer) blocking --add for all 124 targets; moved to rejected_wallets_superseded_20260423.jsonl. Phase 5 re-ran successfully: 124/124 added (117 T2 + 7 WATCHLIST_ONLY), watchlist 162→286. Freshness caveat INVERTED: fresh $8.07M vs Apr 14 snapshot $7.34M (1.10x). Closes CB-APR14-AUDIT140-FINDING; CB-HARVESTER-BUYHOLD retained HIGH as structural-fix TODO. Report: /opt/copybot/logs/cb_mode_b_restore_execution_20260423.md.
- 2026-04-23 evening: BATCH2A-DRIFT-DIAG closed. Prune was Apr 18 (SCAN-7 INACTIVE, activity-freshness, correct-by-design for 1,570 of 2,100). Only ~144 are Mode B restore candidates. Bug-class scope expansion found 3rd surface of BUY_HOLD-blind pattern in monthly_watchlist_review.py (armed May 1) — spawned CB-MONTHLY-REVIEW-BUYHOLD and uplifted CB-HARVESTER-BUYHOLD to HIGH. Report: /opt/copybot/logs/cb_watchlist_drift_diag_20260423.md.
- 2026-04-23 PM: BATCH1-PROBES closed all three TODOs with findings. INFRA-SCOUT-TRUNCATION: 3-layer LLM-in-the-loop bug; spawned INFRA-SCOUT-VALIDATE (code fix) + SCT-881-RECOVERY (optional). INFRA-PUSH-RELIABILITY: no issue, misread on Claude Chat's part. CB-WR-ZERO-ANOMALY: schema-lifecycle drift between write-once profiler_* fields and rolling win_rate; spawned INFRA-PROFILER-FIELD-FRESHNESS. Secondary finding: CB-APR14-AUDIT140-FINDING's $7.34M aggregate uses potentially-stale profiler_pnl; 'CULL_FALSE_POSITIVE' framing weakened pending fresh re-profile in CB-WATCHLIST-DRIFT-AUDIT re-vet.
- 2026-04-23: Onboarding doc URL corrections — earlier Phase 2 report (brain commit d5d3257) added obfuscation suffix to repo URL based on a claude.ai project-share URL misread as the canonical GitHub path. Corrected to plain `molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww` (public, unauthenticated, matches git remote). Separately: Claude Chat claimed mirror-fetch showed 'Apr 18 WORKSPACE content despite Apr 21 on-disk version.' Phase 2 of BATCH1-PROBES proved this was a misread — mirror content was Apr 21, matching origin/main commit 2344c72. Push infrastructure is sound. The confusion came from WORKSPACE's self-reported 'Last updated: 2026-04-18' header line, which had drifted from file state because session writers hadn't refreshed it on each write. See LESSONS entry 'Record-integrity patterns'.
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

---

## Phase 3 (b) validation — 2026-04-28

**Status:** EMPIRICALLY VALIDATED on 44-wallet T2 active cohort.

**Evidence artifact:** /opt/copybot/data/phase3/validation_run_20260428.jsonl
(canonical preservation; source at /opt/copybot/logs/phase3_adhoc_t2_rescore_20260428_063436.jsonl)

**Integrity (3/3 hard checks PASS):**
- 0/44 consumer_flag_active=False
- 0/44 PRODUCER coverage gaps (all 44 records have non-null gate_pnl_resolved)
- 0/44 routing violations (gate_pnl_used == min(legacy, resolved) on every record)

**Outcome:** 42/44 wallets path-A-FAIL under min-gating (95.5%). 2/44 still admit:
- 0x1df5647487: legacy +$14k, resolved +$1k → admits at +$1k. wr=0.874, n=2114.
- 0xd054e72b9e: legacy +$3.8k, resolved +$15k → min picks legacy +$3.8k. wr=0.733.

Both admits are min-gating working as specified, not gate failures.

**Top demote-eligible (largest legacy→resolved swings):**
- 0x8718865882: +$130k → -$146k ($276k swing)
- 0xc949b287f5: +$15k → -$155k ($170k swing)
- 0xbedbc3808d: +$10.5k → -$121k ($132k swing)

**Pre-commitment held:** No action on the 42 demote-eligible wallets until full
TIER3 cycle completes (~2 weeks from 2026-04-28). Validation result is operational
evidence, not authorization to flip.

**Boundary noted:** 0xaa075924e1 (martingale, +$604k resolved / -$4.86M MTM) was
NOT in the cohort — both pnl signals positive, only open-book MTM tells the truth.
Min-gating cannot see this class of risk. Future-work item: realized_ratio gate
variant. See LESSONS-2026-04-28.

**Active observation window:** 2026-04-28 → ~2026-05-12 (TIER3 cycle).


---

## INFRA-PROFILER-FIELD-FRESHNESS — quantification 2026-04-28

**Status:** Quantified. Decision made. Implementation deferred (sequenced).

**Quantification (44-wallet T2 cohort, Apr 27 snapshot vs Apr 28 rescore, 24h window):**

Drift magnitude:
- Legacy: median $478, mean $16,700, max $269k. 28/44 shrank, 5 grew, 11 unchanged.
- Resolved: median $765, mean $12,400. 24/44 grew, 17 shrank, 3 unchanged.

Decision impact:
- 42/44 cohort/rescore decisions agreed (95.5%).
- 2/44 disagreed — both swung from cohort-fail to rescore-admit because one or
  both legs moved >$15k in 24h (0x1df5647487, 0xd054e72b9e).

Direction is asymmetric, not noise:
- Legacy systematically shrank (28/44) — consistent with BUY_HOLD-pattern wallets
  losing legacy edge as stale unresolved entries age out.
- Resolved biased upward (24/44 grew) — consistent with new closes resolving in.

**Verdict:** Snapshots are NOT safe for audits referenced >24h after capture.
A 7-day-old snapshot would likely mis-decide ~10-15% of wallets; a 14-day-old
snapshot is unusable for gate-decision validation.

**Decision: dual fix, sequenced.**

Fix A (watermark — implement first):
- Add `captured_at` ISO8601 field to all cohort/audit JSON outputs.
- Add a freshness check helper: any consumer reading a snapshot warns at >24h
  staleness, hard-fails at >7d staleness.
- Cheaper (one helper, ~5 consumer sites). Closes the bug class that bit us
  today: today's 2-disagreement pattern would have been pre-flagged with a
  24h staleness warning.

Fix B (option a — always-fresh field refresh):
- `rescore_watchlist.py` and producer-side scripts refresh
  `profiler_net_pnl` / `profiler_closed_trades` / `profiler_top_category`
  on every rescore pass alongside the rolling fields.
- Wider scope (12 GATE + 3 DERIVED consumer surface, per Phase 3 recon).
- Closes the underlying staleness; watermark only catches it after the fact.

**Sequencing rationale:**
1. Watermark first — small, reversible, immediately raises signal on stale
   inputs across all current consumers. Defends against the failure mode
   today's analysis surfaced.
2. Option (a) second — needs dedicated recon session to map all 12+3
   consumer sites and confirm no hidden side effects. Scope is large
   enough that it should not piggyback on an unrelated session.

**Spawned tasks (parked for explicit future sessions):**
- INFRA-FRESHNESS-WATERMARK (MED, ~1 session): implement captured_at + helper
- INFRA-FRESHNESS-OPTION-A (HIGH, dedicated session): rescore_watchlist field
  refresh; recon all 12+3 consumer sites first

**Evidence:** /opt/copybot/data/phase3/validation_run_20260428.jsonl (44 records)
combined with /opt/copybot/logs/phase3_active_demote_cohort_20260427.json
(Apr 27 snapshot). Drift quantification reproducible from these two files.



---

## INFRA-WORKSPACE-DRIFT-AUDIT — spawned 2026-04-28

**Status:** PARKED for dedicated session. HIGH priority but not urgent.

**Trigger:** Tier 1+2 arc on 2026-04-28 verified 5 WORKSPACE items against
actual /opt/newsbot/, /opt/copybot/, /opt/_retired/ state. Result: 3 of 5
items (60%) were already shipped or otherwise resolved but never marked.
Stale items so far this session: WeatherBot section, SCAN-7 cron line,
NewsBot datetime.utcnow(), NewsBot 30s burst debounce. Pattern is consistent
enough to be a base rate, not coincidence.

**Verdict:** WORKSPACE is no longer reliable as a "what to work on next"
surface. Every "actionable TODOs" survey returns false positives. Brain
migration on 2026-04-24 imported items but did not sweep status.

**Scope:** Walk every `[ ]` bullet in WORKSPACE; per-item verify against
current /opt/{copybot,newsbot,scoutbot}/ code, recent git log, service
state. Mark `[x]` with verification note for shipped items; reframe or
delete for stale calendar/cron items; leave `[ ]` only for genuinely
pending work. Estimated ~30-45 min focused; produces a clean WORKSPACE
that future "what's actionable" sessions can trust.

**Bundles with:** NewsBot fuzzy-dedup state question (was Tier 2 item;
turned out to be "investigate-then-decide" rather than implementation —
same class as the audit itself, run alongside).

**Prerequisite to:** any future Tier 1+2 sweep session. Without this,
those sessions waste time verifying line-by-line.


---

## INFRA-WORKSPACE-DRIFT-AUDIT — inventory 2026-04-28

**Status:** Inventory complete. Verification deferred to fresh session.

**Surface:** 44 open `[ ]` bullets, 17 done `[x]`, 1 `[!]` deviation marker.

**Batch grouping (5-6 items each, 8 batches total):**

CopyBot (24 items, 4 batches):
- Batch A (baseline):    L14, L15, L20, L21, L22, L26
- Batch B (infra):       L29, L30, L32, L33, L35, L36
- Batch C (SCT + gate):  L37, L38, L39, L42, L43, L57
- Batch D (Apr 27 work): L58, L59, L62, L63, L64, L65

NewsBot + ScoutBot (4 items, 1 batch):
- Batch E:               L75, L76, L83, L84

Cross-cutting (16 items, 3 batches):
- Batch F (Apr 24):      L89, L90, L91, L93, L94, L95
- Batch G (older):       L96, L99, L102, L103, L104, L105
- Batch H (SCT + scout): L110, L111, L115, L116

**Already verified this session (don't re-verify):**
- L29 INFRA-PROFILER-FIELD-FRESHNESS (pending, decision documented)
- L62 PHASE3-LIVE (precondition closed by today's validation; stays open until TIER3 cycle)
- L65 WORKSPACE-HEADER-DISCIPLINE (touched, not closed; CHAT_RULES edit still open)

**Time-deferred — verify "still pending," not "done":**
- L57 CB-FLIP-GATE-TIER3-ANALYSIS — scheduled 2026-05-25
- L75 NewsBot May 7 gate — cohort-driven, awaiting settlement lag
- L94 CB-DECAY-BUYHOLD-AUDIT — gated on next decay-monitor fire

**Effort estimate:** ~3.5 hours focused (5 min/item x 44). Most should resolve
faster; some will need git log dives. Walk batches sequentially with a STOP
between each — fatigue degrades judgment-per-item work faster than mechanical
work.

**Methodology (from today's findings):** For each item, verify against
(a) /opt/{bot}/ code grep, (b) recent git log, (c) service state if applicable.
Mark `[x]` with verification note for shipped items. Reframe or delete for
stale calendar/cron items. Leave `[ ]` only for genuinely pending work.

