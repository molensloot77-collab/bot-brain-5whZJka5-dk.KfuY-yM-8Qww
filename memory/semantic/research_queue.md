# Research Queue

Durable list of open research questions and longer-horizon investigations that aren't actively scheduled but are worth revisiting. Items move here from WORKSPACE when they have no near-term trigger and would bloat the active task list.

Format per entry: source identifier, title, body, **Why:** motivation, **How to apply:** activation trigger.

Created 2026-04-23 from BATCH3A-DEFER-REVIEW.

---

## SCT-AUTO-879 — Anti-patterns playbook

Document the failure modes observed across the bot suite so future sessions can recognize and avoid them. Candidate entries (expand during the write-up):

- **BUY_HOLD-blind bug-class** — four surfaces identified as of 2026-04-23:
  1. `reconstruct_pnl` on 90d USDC transfers (Apr 8 + Apr 14 culls)
  2. SCAN-7 `recent_trades == 0` activity-freshness (Apr 18 prune; correct-by-design)
  3. `closed_trades<30 AND days_active>60` INACTIVE_CULL in `monthly_watchlist_review.py` (ARMED pre-fix; patched in BATCH2B-MODE-B Phase 1)
  4. Stale rejection records in `rejected_wallets.jsonl` after writer fix (`ae89d01`) — surfaced BATCH2B-MODE-B Phase 5
- **Brain-rot** — Claude Chat updating deprecated handover docs instead of WORKSPACE, diagnosed 2026-04-23
- **Schema-lifecycle mismatch** — write-once `profiler_*` fields vs rolling `win_rate` produced the apparent CB-WR-ZERO-ANOMALY
- **LLM-in-the-loop unreliability** — 3-layer truncation chain in INFRA-SCOUT-TRUNCATION; LLM prose hedges not a substitute for syntactic gates
- **Stale-artifacts-of-fixed-bugs pattern** — fixing the writer code path doesn't purge records the old path wrote; downstream caches/registries need sweeping

**Why:** Memory is fresh. Diminishing-returns clock — the longer this waits, the more context evaporates. Higher urgency than SCT-AUTO-878 despite identical scoring.

**How to apply:** Schedule when next session window allows a 1-2 hour synthesis pass. Should reference today's LESSONS.md entries (particularly "Bug-class scope expansion") and the BATCH2A/2B reports. Output: section appended to LESSONS.md under "Anti-patterns" subheading.

**Blocked on:** Nothing — can start immediately.

---

## SCT-AUTO-878 — Cross-bot edges playbook

Synthesize what's producing actual edge across the bot suite (CopyBot, KrajekBot, NewsBot, WeatherBot-retired, ScoutBot). Durable reference for future-self ("what actually works") and new-bot design rubric.

**Why:** Edge attribution is currently scattered across bot-specific session logs and LESSONS fragments. A consolidated view would sharpen decisions on capital allocation, feature prioritization, and new-bot planning. Also helps distinguish "edge we measured" from "edge we assumed."

**How to apply:** Wait for stable cross-bot state — at minimum CB-HARVESTER-BUYHOLD delivered + KrajekBot Phase 2 landings. Revisit ~1 week out (target 2026-04-30 window). Output: dedicated edges.md under semantic/ or appended to STRATEGIC_THREADS.

**Blocked on:** CB-HARVESTER-BUYHOLD + KrajekBot Phase 2.

---

## SCT-AUTO-903 — Lead-lag methodology for cross-market analysis

Study causal relationships between related markets (e.g. BTC up-down 5m vs 15m vs 1h). Open research question with no operational dependency.

**Candidate techniques:**
- Granger causality testing (SCT-AUTO-904, merged here) — statistical test for directional predictive power between time series.
- Transfer entropy — non-linear alternative.
- Cointegration — for markets that should move together.

**Why:** Some of the edge we observe in CopyBot may reflect crowd-lead wallets whose signals predict market movement before price reflects it. A principled framework would help distinguish genuine lead from correlated noise, and could inform MON-30 flow-spike pre-filtering.

**How to apply:** One-off research session. Start with a small pilot: pick 2-3 related Polymarket markets with sufficient resolution history, run Granger tests pairwise, report whether the directional relationships are interpretable. No commitment to integrate — this is an investigation, not a build. If results are interesting, escalate to a proper MON-30 design doc.

**Blocked on:** Nothing operational. Deprioritized vs INVESTIGATE items but worth keeping around.

---

## Notes on this file

- Moving items here is a **defer-with-context** action, not a drop. Items should be revisited at their trigger, not forgotten.
- When activating a research-queue item into real work, remove it from this file and add a WORKSPACE TODO with concrete next step. Record the activation date in the commit.
- Keep entries brief enough to scan in under a minute each. If a rewrite is turning into 500+ words, it's probably ready to be a proper TODO or a semantic artifact in its own file.
