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
- **Bug-class scope expansion.** When a bug is identified, postmortem the bug class, not the incident. Enumerate every code path, scheduled job, or downstream artifact that consumed the bug-class's output during its active window. Treat unknown-status surfaces as suspected-affected until proven safe — silence is not evidence of safety. Surfaced 2026-04-23: Apr 8 cull postmortem caught BUY_HOLD-blind `reconstruct_pnl()` at top-20 scope (10 restores landed Apr 17). Same bug ran on Apr 14 at 140-wallet scope and was missed for 9 days because the kill-switch alert fired on aggregate WR, not on the specific cohort. $7.34M of falsely-demoted edge sat untouched. The loudest failure is rarely the only failure.

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

---

## 2026-04-22 — CopyBot execution audit: sizing ceiling finding

**What we discovered:**
Execution audit on 13,351 signals (paper + live combined) revealed CopyBot
sizes at median 1.29% of source wallet bet size. Our median bet: $0.31.
Their median bet: $28.80. Outcome correlation with source = 100% (same market,
same direction), but P&L correlation = 0% because sizing magnitude makes
our P&L mathematically capped at 1.3% of source P&L.

17 days of live trading, 4 weeks of paper gate work, all operating under this
ceiling. Gate quality, slippage, latency, selection tuning — all bounded by
the sizing architecture regardless of correctness.

**Load-bearing numbers:**
- Median size ratio our/their (paper+live, n=13,351): 1.29%
- Live-only sub-sample ratio: 1.25% (paper and live use same code path)
- Median our_size_usd: $0.31
- Median their_size_usd: $28.80
- NOISE_BET skips: 13,178 (63.7% of all rejections)
- CROWDED_SKIP: 7,508 (36.3%)
- Slippage cost total across 161 live settled trades: $15.29 (trivial)

**Process lessons (reusable for any copy/signal bot):**

1. **Measure source-comparative performance, not absolute performance.** For
   any copy-based strategy, the primary metric is "what % of source edge did
   we capture," NOT "are we positive." 51.9% WR looked acceptable; 0% P&L
   correlation on 100% outcome correlation was the real story.

2. **Verify code changes against live effects, not commits.** CB-SIZING-FIX on
   Apr 15 raised MIN_LIVE_BET_USD=1, commit landed, compile OK. Median live
   bet size on subsequent fills remained $0.31. Code change ≠ observed effect.
   Always pull post-deploy distribution to confirm the intended behavior.

3. **When paper exactly matches live, one of them is lying.** Paper and live
   had identical sizing ratios (1.25% vs 1.29%). Either paper over-simulates
   realism (unlikely) or live execution bypasses something paper should have
   (the case here). Divergence is a signal; exact match is too.

4. **Ceiling analysis before tuning.** Before tuning filters / thresholds /
   models, compute the theoretical maximum output of the system given current
   architecture. 2.8 signals/day × $3 bet = $16/month WeatherBot ceiling —
   same class of finding. Ceiling = architecture; tuning only gets you
   closer to it.

5. **Decomposed audits beat single headline numbers.** Splitting "capture %"
   into three audits (execution leak, sizing gap, selection leak) revealed
   sizing was the dominant lever. A single "edge capture %" number would
   have masked which dimension to fix.

**Forward implications:**
- CB-FLIP-GATE shadow timer: continue observing, but do not expect outcome
  impact at current sizing. Gate quality is bounded by sizing architecture.
- CB-BACKTEST-TIER3: DEFER. Backtesting gate quality on a system clipped to
  1% size won't reveal what gate quality is worth at full size.
- CB-BACKTEST-RES-TS: demote HIGH → MED. Not urgent until sizing fix.
- CB-SIZING-OVERHAUL: TOP PRIORITY. Investigation before any code change.


---

## 2026-04-22 (late) — NOISE_BET validation correction + cascade pattern

### Correction to earlier entry ("CopyBot execution audit: sizing ceiling")

Earlier today I wrote that NOISE_BET "correctly discriminates losing bets"
based on the observation that 832 NOISE_BET-tagged signals had source P&L
of −$252K. That reasoning was wrong.

**The methodology error:**
The NOISE_BET filter fires on ~100% of CopyBot signals from Apr 6 onward.
There is no non-firing control group. Claiming "NOISE_BET selects losers"
from a sample where 100% of signals are labelled NOISE_BET is not a filter
discrimination finding — it's just the time-distribution of the overall
cohort's source P&L dressed up as filter performance.

The −$252K loss was real. Its attribution to NOISE_BET was not. Source
wallets lost money during Apr 13-22 regardless of tag, and 100% of signals
in that window happen to be tagged NOISE_BET, so the loss appeared under
the NOISE_BET bucket by definition.

**The methodology lesson (reusable):**
> Before evaluating a filter, verify the filter has a non-firing control
> group of non-trivial size. If 0% or 100% of samples pass the filter, the
> filter isn't filtering — it's labelling. You cannot measure discrimination
> on a sample with no contrast.

Specific check for any future filter validation: split the sample by
filter-fired vs filter-not-fired, confirm both groups have n ≥ 30 on at
least 3 distinct days, THEN compare outcomes. If either group is empty or
temporally concentrated, the filter verdict is unreliable.

**How the correction surfaced:**
Three-cut time-split analysis on Apr 22 revealed NOISE_BET rate went from
0% (Mar 24-Apr 5) to 95-100% (Apr 6 onward). Rate stdev 41.2pp across
days, range 0-100%. This is not a filter firing on marginal signals —
it's a regime switch triggered by something (likely wiring activation of
check_market_relative) around Apr 6.

---

### Pattern: cascading defensive failures

Three independent safety mechanisms, each individually reasonable, each
assuming the others worked. The bug emerged from their interaction.
Nobody had traced the full chain end-to-end before today.

**CopyBot sizing ceiling, root-cause stack (Apr 22):**

| # | Mechanism | Intent | Actual behavior |
|---|-----------|--------|-----------------|
| 1 | NOISE_BET_RATIO = 0.001 threshold | Fire only on thin markets | Fires on ~100% of signals at $1 bet size |
| 2 | 0.5× multiplier on NOISE_BET | Downsize thin-market positions | Downscales ALL signals to half watchlist config |
| 3 | MIN_LIVE_BET_USD = 1.00 floor | Enforce minimum bet size | Declared but never re-applied after multipliers |

Each mechanism individually would have caught the bug if the others
failed:
- If threshold was right, multiplier wouldn't fire on 100% of signals
- If multiplier was skip-not-scale, floor would be moot
- If floor was re-applied post-multiplier, sub-floor bets wouldn't land

All three had gaps, and the gaps compounded. Median live bet size $0.38
despite $1 floor and $1 watchlist config.

**Pattern lesson (reusable):**
> When a system has multiple defensive mechanisms at different layers
> (threshold gates, scaling adjustments, minimum floors), a single read-
> only end-to-end trace of actual values through all layers is worth more
> than N layers of code review. Each layer's author assumes the others
> are doing their job correctly. Trace the numbers, not the intent.

**Forward rule:**
For any CopyBot sizing or filter change: pull the last 100 SIGNAL records,
show the intermediate values at each decision point, verify the observed
final value matches the intended formula. "CB-SIZING-FIX landed" and
"median bet size is $1+" are not the same claim.

---

## Record-integrity patterns

* **Schema-lifecycle mismatch.** When a record contains fields written at different times by different code paths, those fields can describe different temporal states of the same entity — and consumers of the record may silently mix them. Surfaced 2026-04-23: CopyBot watchlist records hold `profiler_net_pnl` (write-once at harvester.py add-time) adjacent to `win_rate` (refreshed on every rescore_watchlist.py pass). APR14-AUDIT-140's static scan used both fields as if they described the same Apr 14 state; they didn't. Fix-class: either refresh all fields on every write pass, or name fields to signal their freshness contract (e.g. `profiler_net_pnl_at_add` vs `win_rate_rolling`). Prompt-side mitigation: when reading a record with multi-source fields, verify write contracts before treating fields as a coherent snapshot.

* **File state vs content assertion.** A file's self-reported metadata (header dates, "Last updated" lines, version strings, README dates) is not the same as its filesystem state (mtime, git log). They can disagree, and when they disagree, that disagreement is itself a signal worth reporting explicitly. Surfaced 2026-04-23: Claude Chat read WORKSPACE.md's `Last updated: 2026-04-18` header line and reported "mirror serving 5-day-old content," when the file was actually Apr 21 on both disk and mirror — only the header had drifted from file state. Session writers had been appending bullets without refreshing the header. Two rules: (1) when evaluating staleness, check both file state and content assertions, and note disagreement; (2) when writing to a file with a "Last updated" header, refresh the header as part of the write — otherwise the header becomes a misleading artifact that future sessions will misread.

* **LLM-emitted commands need syntactic validation before they land.** When an LLM produces machine-executable output (shell commands, API calls, SQL), prose hedges in the output text are not a substitute for a post-generation gate. Surfaced 2026-04-23: ScoutBot's auto_ingestor.py emitted `harvester.py --add 0x594edb91` (10 chars, invalid) alongside the hedge "(full address required)" — the LLM caught the problem, then wrote the row anyway. A 42-char hex regex between generation and persistence would have blocked it. Prompt engineering compounds unreliability; syntactic gates don't.

* **Counter semantics depend on the window over which they're computed; name fields to expose that dependence.** When a metric is computed over a bounded window (trade history at max_pages=N, time-slice at last N days, top-N candidates), the metric's value is a function of both the underlying signal AND the window size. Two calls with different N can produce different values for the same underlying entity, and neither is wrong — they're answers to different questions. Field names should surface the window parameter (`_at_depth_N`, `_in_last_N_days`, `_of_top_N`) rather than hiding it, so consumers know which slice they're reading. Surfaced 2026-04-24 via CB-HARVESTER-BUYHOLD-MAXPAGES: the profiler_attr_proxy_fallback_hits counter at max_pages=6 is partly measuring truncation-artifacts (old close trades out of window leaving orphan BUY trades that look still-open); at max_pages=200 it measures real proxy/EOA mismatches almost exclusively. Same counter name, two different phenomena. Fix: emit the depth alongside the counter so consumers can reason about what the value means. Generalizes to any truncated-lookback metric: win_rate over last 30d vs all-time, top-N crowd-consensus metrics, percentile-bounded effect sizes.

* **systemd drop-ins and unit files activate on the next daemon-reload anywhere on the system, not on an intended "activation" action.** Writing a file to `/etc/systemd/system/<name>.service.d/` or `/etc/systemd/system/` and intending to run daemon-reload later is functionally indistinguishable from activating immediately, because any unrelated session that edits any other unit and runs daemon-reload will pick up the pending file. "Staged, not activated" is a mental model that breaks at the first cross-session unit edit, not a safety state. Two rules: (1) treat a drop-in or unit file landing on `/etc/systemd/system/` as live; activation timing is determined by the next daemon-reload from any source, not by human intent. (2) When the activation moment matters (e.g. coordinating with an upstream run, confirming cohort freshness), gate on the service's next start — timer fire, manual start — not on daemon-reload. A drop-in that shouldn't be active must stay outside `/etc/systemd/system/` (e.g. in `/tmp/` or a staging directory) until the actual activation moment. Surfaced 2026-04-24: CB-HARVESTER-BUYHOLD Phase 2 drop-in at /etc/systemd/system/copybot-rescore.service.d/buyhold-attr.conf (mtime Apr 24 08:49 UTC) was merged into effective config by an unrelated CB-FLIP-GATE-TIMEOUT-V2 daemon-reload mid-afternoon the same day; the "explicit BigW activation" daemon-reload that evening was a no-op. Same pattern as CB-FLIP-GATE shadow timer (staged at /tmp/ on Apr 21, moved to /etc/systemd/system/ on Apr 22, asserted "not enabled" in WORKSPACE until 2026-04-24 while actually firing nightly since Apr 22).

* **Content-addressed SHA over session-handoff narration.** When a message claims a file's content changed (commits landed, "your stale copies," "rewritten today") but the content-addressed SHA returned by the GitHub Contents API is unchanged, trust the SHA. File blob SHAs are content-derived — if a single byte differs, the SHA differs. Equal SHA on the same `?ref=main` query means the file on `main` is byte-for-byte identical to the prior fetch, regardless of what any narration asserts. Surfaced 2026-04-25: a session re-orientation message claimed three commits had touched WORKSPACE.md and LESSONS.md after an initial fetch and instructed a re-fetch as "stale." Both API responses returned identical SHAs (00436aa... and 4b9084d...) and identical sizes. Investigation deferred to BigW (commits may have existed on a non-main branch, or touched other files entirely). Two rules: (1) when a handoff message and the API disagree, the API wins for "what is on main right now"; raise the discrepancy explicitly rather than re-fetching as if stale. (2) Session-handoff prose is human-memory-derived and subject to drift between events ("which files did I edit") and recall ("which files I claim I edited") — same failure mode as the Apr 23 "File state vs content assertion" lesson but inverted: there the file content lied about its own freshness; here the narration lied about the file's freshness. Generalizes: any time a content-addressed identifier (git blob SHA, content hash, ETag) is available alongside a narrative claim, the identifier is the cheaper and more reliable signal.


---

## Retirement methodology

### 2026-04-24 — Retirement requires three actions, not one

When retiring a service or component, the operation is not "stop" — it is "stop AND disable AND remove unit file." Surfaced 2026-04-24: the Apr 8 PolyArb retirement stopped polybot.service but left polybot-paper.service, polybot-arb2.service, polybot-delta-exec.service, polybot-tgnotify.service, polybot-watchdog.service in `enabled` state. They were inactive at runtime but would have auto-started on the next reboot. INFRA-RETIRE-CLEANUP caught this 16 days later only because Phase 0 audit explicitly listed `is-enabled` per unit — `is-active` alone would have shown them all clean.

**Reusable rule:** "Stopped" describes runtime state. "Enabled" describes boot state. A retirement that only addresses runtime state is incomplete and creates a latent reactivation hazard. Standard retirement checklist:
1. systemctl stop <unit>
2. systemctl disable <unit>
3. rm /etc/systemd/system/<unit>
4. systemctl daemon-reload
5. (Document in retirement notice + brain.)

**Generalizes to:** anywhere "active state" and "configured state" are tracked separately — cron commented vs deleted, scheduled job paused vs removed, feature flag set false vs feature code deleted. Always sweep both.

### 2026-04-24 — Build shared infrastructure with names that won't lie

When one of N bots eventually retires, any shared infrastructure named after that bot becomes a structural lie. Surfaced 2026-04-24: `/opt/polybot/venv/` is the shared Python venv for CopyBot, NewsBot's venv parent, hub healthcheck, and ~25 systemd units + crons. `polybot-harvester.service` is actually CopyBot's harvester. Both were named when PolyArb was the lead bot; both became misleading the day PolyArb retired.

**Reusable rule:** Shared infrastructure (venvs, environment files, common config dirs, generic services like a harvester or a notifier) goes under generic names from day one — `/opt/_shared/venv/`, `/etc/systemd/system/<actual-consumer>.service`. Bot-specific names go to bot-specific resources only. The discipline pays off the first time any bot retires.

**Cost of getting this wrong:** an INFRA-VENV-MIGRATION TODO + an INFRA-SERVICE-RENAME TODO that someone has to schedule and execute later, or live with permanent confusion in onboarding.

### 2026-04-24 — Audit the file name, not just the bot name

Phase 0 audit of INFRA-RETIRE-CLEANUP greped for `weatherbot|krajek|polybot|polyarb` strings across active surfaces. It missed three runtime readers/writers of `AllBots_TODO_CURRENT.md` in `/opt/scoutbot/` because the audit looked for retired-bot *names*, not the file *name* about to be deleted. Caught at the pre-deletion grep gate (Task 1.5), required a mid-session insert (Tasks 1.4e–1.4i, ~30 min) to redirect ScoutBot to `SCOUTBOT_INBOX.md`. Without the gate, ScoutBot's nightly run at Sat 02:00 UTC would have crashed, generating phantom morning-brief alerts and potentially data loss in the SCT-AUTO promotion path.

**Reusable rule:** When auditing for "what depends on X before deletion", grep for X's *file name* and *path*, not just for the *concept* X represents. The two greps surface different sets. Concept greps catch usage in code logic; path greps catch readers/writers.

### 2026-04-24 — Feature-flag execution as a Phase 0 alternative

When a structural fix has multiple surfaces of risk (e.g. CB-HARVESTER-BUYHOLD: 4 known BUY_HOLD-blind surfaces in 16 days), feature-flag execution offers a natural Phase-0 analog that doesn't require a separate investigation session:

- Producer writes new data; no consumer changes
- Natural observation window (next cron pass, batch cycle, etc.)
- Diagnostic report compares old-vs-new in production
- Consumer flip gated on review of that diagnostic

The equivalence: Phase 0 audit = "what WILL the change affect?" Feature-flag observation = "what DID the change produce?" Both answer the same class of question (does the planned change do what we think?) but the flag gives ground truth against production data, where audit gives projection. Use audit when you can't test in production safely (destructive changes, irreversible moves, cost-committed operations). Use feature-flag when you can run producer + consumer in parallel.

Caveat: feature-flag requires discipline. The flag must stay OFF by default; the diagnostic must actually run; BigW must actually review before the flip. Skipping any of these collapses back to a direct consumer change with extra steps.

Also: the flag's existence must be discoverable (AGENT.md + rescore.log line), or future sessions forget the system has a flag and make direct changes underneath it.

### 2026-04-24 — Acceptance-test sanity expectations need to match the bug being fixed

Phase 1 design report Task 2.1.6 said: "pair-matched-rich wallet: resolved_pnl should roughly equal existing profiler_net_pnl (they measure similar things for this wallet type)." Phase 2 acceptance test on a real pair-matched-rich proxy wallet (8,647 closed pair trades) showed profiler_resolved_pnl=$2,158 vs profiler_net_pnl=$62,268 — a 30× gap. Initial reaction: "bug in implementation."

Investigation showed it was correct behavior: the wallet has many unmatched-buy positions (held to resolution, lost; never recorded as SELL in /activity stream). Legacy pair-matched scoring is BLIND to those held-to-resolution losses. The new attribution catches them via the per-cid /markets fallback. The wallet's true edge IS $2k, not $62k.

This is exactly the BUY_HOLD-blind bug class the fix addresses. The Phase 1 sanity-check intuition was wrong: pair-matched-rich wallets can have very different new vs legacy P&L if they hold-to-resolution at all. The correct sanity check: profiler_realized_ratio + open_positions count + proxy_fallback_hits together tell whether the wallet has structurally different P&L profile than the legacy scorer can see.

**Reusable rule:** when writing acceptance tests for a fix, the sanity expectations must NOT assume the bug doesn't exist. Test outputs that "look like the legacy" prove the new implementation copies the bug. Test outputs that DIVERGE from legacy in the predicted direction prove the new implementation surfaces what was hidden. Build the divergence into the test: pick at least one wallet where you expect the new and old to disagree, and verify they do.

### 2026-04-24 — Mirror freshness: raw CDN vs API

"Brain mirror = sole source of truth" has one failure mode: reads within ~5 min of a write can return stale content. `raw.githubusercontent.com` caches unauthenticated raw-file responses at edge nodes with variable TTL. Different edges can show different versions during the window.

**Reusable rule:** for fresh-push-then-read scenarios (session close → session open), use the GitHub Contents API (`api.github.com/repos/.../contents/...`), not raw URLs. The API returns base64-encoded content with commit SHA metadata; it's cache-bypassed (or uses much shorter TTL) and includes freshness evidence. Raw URLs are for bulk reads where a 5-min staleness window is acceptable.

**Generalizes to:** any "eventually-consistent CDN with aggressive edge caching" — GitHub raw, S3 static hosting, Cloudflare Pages, Netlify Functions cache. If the read happens seconds-to-minutes after a write, expect to see stale content from a fraction of requests. When correctness matters, read from the source-of-record API, not the edge-cached mirror.

**Surfaced 2026-04-24:** session close pushed 3 brain commits (d0429fa / 0855d57 / 9168330). New Claude Chat session opened within ~5 min, fetched WORKSPACE from raw URL, got 2026-04-23 content — missing all three today commits. `git log origin/main..HEAD` confirmed pushes landed cleanly. Pure edge-cache lag. No infrastructure failure.

**Fix applied same day:** session_start_paste.md + CLAUDE_CHAT_ONBOARDING.md updated to use the Contents API. Raw URLs preserved as rate-limit fallback (Contents API is 60/hr unauthenticated).

### 2026-04-24 (late) — Correction to INFRA-PUSH-RELIABILITY postmortem

The Apr 23 INFRA-PUSH-RELIABILITY postmortem asserted that the canonical brain-repo URL is `molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww`, and treated an earlier Claude Chat write that added an obfuscation suffix to the URL as an error to be corrected. Both conclusions were wrong.

Actual state: BigW had intentionally renamed the repo on GitHub for privacy-through-obscurity before the Apr 23 session. The new canonical is `bot-brain-5whZJka5-dk.KfuY-yM-8Qww`. `bot-brain` is a legacy-redirect name preserved by GitHub. The "obfuscation suffix" Claude Chat added was the LLM correctly reading the post-rename canonical; the Apr 23 correction that stripped it was a regression disguised as a fix.

**Reusable lesson:** Before postmorteming a URL as "wrong", verify the current state of the resource with the owning authority (here: GitHub's HTTP response on the old name should have surfaced a 301 during the Apr 23 session). A confident-sounding "we know the canonical" without a live check is the same class of failure as schema-lifecycle mismatch — we asserted a fact from memory when the authority had moved on.

Also: privacy-through-obscurity only works if the obscure name is the only one used. Leaving the guessable name in docs as a "fallback" or for "readability" broadcasts exactly the thing the rename was meant to hide. Surface-area hygiene matters.

Surfaced 2026-04-24 when the rename was reconfirmed as intentional. Fix applied same session: all active URLs in /root/.agent/ updated, brain git remote updated, session_start_paste.md + CLAUDE_CHAT_ONBOARDING.md updated with an explicit "do not revert" note.

## 2026-04-24 (late) — Pre-commit pass/fail criteria BEFORE observing outcome data

For any evaluation where pass/fail verdict matters (gate validation, paper-to-live promotion, strategy go/no-go), the criterion must be locked before the outcome data is visible. Not "drafted before" — locked, with specific thresholds, specific effect sizes, specific statistical tests, specific cohort-size requirements. Committed to WORKSPACE with checkbox open, referenced by ID from the session that later consumes the data.

Reason: once the data is visible, human reasoning drifts toward interpretations that confirm whichever way the data leans. "It's a pass because p=0.07 which is close enough" or "it's a fail because the effect size was only 1.3x and I wanted 1.5x" are both rationalized decisions that the pre-commitment would have rendered clean.

Mechanism: pre-commitment is a WORKSPACE entry with the checkbox [ ] open, containing all specific numbers (thresholds, effect sizes, p values, cohort minimums). The analysis session that consumes the data cites the TODO by ID BEFORE any computation begins. If the criterion needs loosening after seeing the data, that's a second decision requiring its own pre-commitment and explicit BigW approval — not a quiet drift inside the analysis session.

**Surfaced 2026-04-24 late:** CB-FLIP-GATE-TIER3-CRITERIA locked before Saturday 02:30 UTC gate-fire. Forward window 30d, source-wallet P&L measure, Mann-Whitney p<0.05 + 1.5x-or-sign-flip effect size, >=30 per cohort at each of 5 thresholds, >=3 of 5 thresholds pass. Null verdict retires the apparatus, not tunes it. Sunday analysis session must cite this TODO's criterion before computing anything.

**Generalizes to:** any paper→live gate, backtest verdict, filter validation, or wallet-cull decision. Write the pass bar before running the numbers. If the pass bar feels arbitrary writing it, that discomfort is the signal that you don't actually know what pass means — figure that out BEFORE computing, because afterward you'll know what the data says and convince yourself that was always the bar.

Related: LESSONS "NOISE_BET validation correction" (2026-04-22) is the same class of failure from the other direction — interpreting filter performance on a sample that doesn't support the interpretation. Pre-commitment would have caught that by requiring a non-firing control group in the pass criterion.

## 2026-04-24 — Repo-name discipline + GitHub redirect leak

Hardened repo names (security-through-obscurity for public repos containing
sensitive content) have one structural failure mode: GitHub auto-redirects
from any prior name to the current canonical name. The redirect is HTTP 200
with the canonical URL in the response — so a single `curl -I` of a bare
pre-hardening URL leaks the hardened name to anyone who finds the bare URL.

**Reusable rule:** server-side discipline is the only place obfuscation can
be enforced. Audit and redact every artifact on the server that could leak
the bare/pre-hardened form. Accept that public git history (commits, old
pushed content) cannot be retroactively cleaned without rewrite-and-force-
push, which itself signals "interesting repo here" to anyone watching.

**Threat-model honesty:** for public GitHub repos, name obfuscation is
hygiene, not defense. Real security comes from:
- No private keys / API secrets in the repo
- No content that becomes weaponizable when paired with the public-action
  data already on-chain
- Treat the obfuscation as raising the cost of casual discovery, not
  blocking determined inspection

**Pattern for runtime references:** runtime scripts (cron, services) read
the URL from `git -C <repo> remote get-url origin` — single source of truth
in `.git/config`. Hardcoded URLs in scripts mean URL rotation requires a
multi-file edit; indirection means it's a one-line `git remote set-url`.
Static docs (LESSONS, MORNING_BRIEF, session-start paste) keep hardcoded
URLs because their purpose is human-readable copy-paste.

**Surfaced 2026-04-24:** session_start_paste.md update (commit 6dc7710,
INFRA-BRAIN-MIRROR-FRESHNESS) introduced bare-URL references during
the API-vs-raw fix. Audit found 16 bare references across 7 files,
including the morning.sh cron fetcher (which was succeeding via redirect,
silently). Fixed in INFRA-BRAIN-REPO-NAME-AUDIT same day.

**Bug class:** every previous "fix" today (INFRA-RETIRE-CLEANUP, INFRA-
BRAIN-MIRROR-FRESHNESS) introduced or preserved bare-URL references that
the producer was content to ship and the consumer was content to redirect-
follow. Pattern: when the wrong-but-functional path silently works, the
bug accumulates across multiple sessions until something forces explicit
inspection (here: the API rate-limit hitting on the 4th distinct file
request, exposing the redirect chain).

## 2026-04-24 — Audit greps must bypass ignore-files

Surfaced during INFRA-BRAIN-REPO-NAME-AUDIT Phase 0: the initial `grep -rIn`
call was actually routed through a shell wrapper using ugrep with `--ignore-files`
behavior. /root/.gitignore excludes `docs/` and `.claude/`. Result: Phase 0
silently undercounted bare references — files in /root/docs/ were invisible
to the audit until the wrapper was bypassed.

**Reusable rule:** for audits — anything where the question is "where does
X appear in this filesystem?" — use `command grep` (or `\grep`, or an
explicit absolute path) to bypass aliases and shell wrappers that may
apply ignore-file behavior. Tools that respect .gitignore (ugrep,
ripgrep with default config, git grep) work correctly for development
greps but hide content from audits.

**Generalizes to:** any tool that has a "be smart about what's interesting"
default behavior. Audits explicitly want every match, including in files
the development workflow has classified as boring.

**Concrete pattern for audits:**
  - `command grep -rIn 'pattern' /root /opt /etc 2>/dev/null`
  - or: `find ... -exec grep ...`
  - NOT: shell-aliased `grep` if any alias or wrapper exists

**Test the audit's audit:** before declaring scope complete, sanity-check
the count by running with a different tool (find + grep, or python with
glob) and comparing. Discrepancies between two methods reveal exactly this
ignore-file class of bug.

**Bug-class:** silent undercount in audits. Adjacent to "the file state
is not what its self-reported header says" (2026-04-23 Record-integrity
patterns) — same family of "tool defaults make the right answer harder
to find than the wrong one."

## 2026-04-25 — `systemctl start <timer>` after suppression replays missed fires when `Persistent=true`

A timer unit with `Persistent=true` records each missed scheduled fire and,
on the next `start`, executes a catch-up immediately for the most recent
missed fire. This is correct behavior for the "machine was off, run what we
missed" use case, but is wrong for the "suppress this one fire, resume normal
schedule next cycle" workflow.

Surfaced 2026-04-25: copybot-flip-gate-shadow.timer was `stop`'d Friday to
suppress Sat 02:30 UTC fire (after the manual 10:55 UTC run brought work
forward); Saturday `start` at 09:56:38 UTC immediately fired the catch-up
the suppression was designed to avoid. Snapshot artifact preserved (taken
24 seconds before resume), so the cohort-freeze goal survived; the
suppression goal did not.

**Two rules:**

1. When planning to suppress a single timer fire and resume, check the unit
   for `Persistent=` first — `false` (or absent, default `false`) makes
   start-after-stop safe; `true` makes it equivalent to running the missed
   fire on resume.
2. "Stop the timer" and "skip this fire" are not the same operation under
   `Persistent=true`. Correct ways to skip a fire under `Persistent=true`:
   `systemctl mask <unit>` for the window then `unmask` after the scheduled
   time has passed (the masked unit can't run, so the missed-fire record
   gets cleaned by the next `OnCalendar` evaluation), or run the service
   manually inside the suppression window so the fire is "satisfied"
   not "missed."

**Generalizes to any catch-up-on-resume scheduler:** cron with `anacron`
semantics; k8s CronJobs with `concurrencyPolicy: Allow` and a
`startingDeadlineSeconds` longer than the suppression window; etc.

## 2026-04-25 — Verify systemd unit cadence before asserting "next fire" timing

When drafting a prompt that commits to a specific next-fire timestamp ("next
fire = 2026-05-02 02:30 UTC"), the cadence (`OnCalendar=`) and the catch-up
policy (`Persistent=`) of the timer must be read from the unit, not inferred
from prose memory of the workflow.

Surfaced 2026-04-25: a Saturday-sequence prompt asserted next fire would be
one week out from a suppressed Saturday fire ("weekly schedule"). The actual
unit was `OnCalendar=*-*-* 02:30:00` (daily) with `Persistent=true`, so the
prompt's stated expectation diverged from the unit's behavior in two
independent ways.

Same failure-mode family as 2026-04-24 "systemd drop-ins activate on the
next daemon-reload anywhere" — systemd's behavior is determined by the unit
file, not by what the operator believes about the unit.

**Rule:** any prompt that includes a "verify next fire = <timestamp>" check
must derive that timestamp from `systemctl cat <unit>` plus the `Persistent=`
setting plus current time, not from prior session prose. The verification
step protected this session — `list-timers` returned `infinity` rather than
the asserted 2026-05-02 because the catch-up fire was already running.
Without that verification, the prompt would have asserted timing that did
not match reality.

## 2026-04-25 — IDE caches flood audit greps; same family as gitignore'd-dirs-hidden-from-greps, opposite direction

Audit greps suffer from two symmetric scope-divergence failures: the
intended scope can be smaller than the search hit-set (noise floods
the result), or larger than the search hit-set (real references hide
in skipped paths). Both failures are silent — the grep returns "answers"
that look authoritative but aren't.

Surfaced 2026-04-25 (HUB-ORPHAN-AUDIT, follow-on to 2026-04-24
INFRA-BRAIN-REPO-NAME-AUDIT lesson "audit greps must bypass
ignore-files"): a reference scan for `wallet_monitor` across `/opt/`
and `/root/` returned heavy noise from VS Code's `.vscode-server/data/`
IDE caches — file-content snapshots and search indexes that are
neither source of truth nor live infrastructure. The real references
(`patch_mon5`, `patch_scan11`, AGENT.md rule) were buried in cache
hits. Operator filtered manually to find them.

The Apr 24 lesson was: respect-gitignore caused audit greps to skip
real source paths (`memory/working/SCOUTBOT_INBOX.md` was gitignored,
hid bare repo references). Today's failure is the inverse: a search
that didn't filter IDE caches drowned real references in build
artifacts and editor metadata.

**Two-direction rule:**

1. **Audit greps must include real source paths the operator might have
   excluded** (gitignore, .ripgrepignore, find -prune defaults). Use
   `--no-ignore` or equivalent.
2. **Audit greps must exclude paths that are not source of truth** (IDE
   caches, build artifacts, language-server indexes, virtualenv site-
   packages, node_modules, `__pycache__`, `.git/`). Use targeted
   `--exclude-dir=` or restrict to known-source directory roots.

Symmetric failure modes need both rules. A grep that gets one right
without the other is still wrong. Concretely for this codebase:
grep -rn --no-ignore 
--exclude-dir=.git --exclude-dir=pycache 
--exclude-dir=.vscode-server --exclude-dir=node_modules 
--exclude-dir=venv --exclude-dir=_retired 
'<pattern>' /opt/ /root/ /etc/systemd/ /etc/cron.d/ /var/spool/cron/

The exact exclusion list will vary; the principle is that audit greps
need explicit scope on *both* ends.

**Generalizes to:** any tool that returns "matches across a directory
tree" — find, fd, ag, ripgrep, sourcegraph queries, IDE-wide find. Same
class for code-search via LSP or Sourcegraph: the index covers what it
covers, which is rarely exactly what the audit intends.

## 2026-04-28

**GitHub Contents API serves stale blob SHAs independently of `?ref=main` cache-bypass intent.** Falsifies the 2026-04-24 LESSONS entry "Mirror freshness: raw CDN vs API" which framed Contents API as the cache-bypassed fresh path. Empirical: Tuesday 2026-04-28 ~05:00 UTC, Contents API returned WORKSPACE.md SHA `00436aada8` (the Friday 2026-04-24 version) despite origin/main being at `c55b46c` (Monday evening auto-commit) and the file's actual current blob hash being `aefc3b67`. The same fetch URL via raw.githubusercontent.com with a cache-buster query param (`?cb=$(date +%s)`) returned the current Apr 27 content with HTTP 200 and a 5-min CDN window etag. **Working bypass pattern: `https://raw.githubusercontent.com/<repo>/main/<path>?cb=<unix_ts>`.** Contents API has its own cache layer separate from the raw CDN; the cache-buster trick only works on raw URLs. Generalizes: any "this API path is cache-bypassed" claim should be empirically verified by SHA-cross-check, not taken as documented behavior — GitHub's caching layers are not transparently consistent.

**File structure assumptions cost time when based on partial reads.** Tuesday morning ate two minutes diagnosing a phantom "yesterday's LESSONS entries didn't land" alarm because I'd assumed LESSONS was dates-first ordered and used `head -50` to grep for `## 2026-` headers. The actual file has a categorical reference section at top, with dated entries appearing later. Companion to the 2026-04-27 entry on brain path references: when about to edit a file, sample at multiple positions (`head` + `grep -n` + `tail`) rather than relying on the first N lines to represent overall structure. Same root cause: operating on partial knowledge of a file's organization.

**Hardening blocks need MORE pre-run scrutiny than usual, not less.** Tuesday afternoon a pre-(b) cleanup block — whose entire purpose was to fix a silent-grep-failure (the `/opt/scoutbot/SCOUTBOT_INBOX.md` path bug from earlier the same day) — itself contained two fresh instances of the same bug class: (1) targeted `/root/docs/session_start_paste.md` when the authoritative path per CLAUDE.md is `/root/.agent/session_start_paste.md`, (2) used a single-format awk filter that would have silently missed 95 of 97 table-format items in the inbox, producing a false-clean bulk-review marker that would have licensed future sessions to skip re-scanning. Caught by pre-run review before execution. Pattern: confirmation bias is highest exactly when writing a fix — "I am defending against the silent-failure mode, therefore my defense is correct" — which obscures fresh instances of the same bug class living in the defense itself. Defense: every block whose purpose is to harden against a silent-failure mode must verify (a) every path it targets actually exists, (b) every filter format matches the actual data shape, (c) the abort/output path is loud rather than silent if its assumptions are wrong. The discipline is recursive: the meta-check is itself susceptible to the same bias, so the artifact under review should be inspected by an independent pass — different reader, different time, or both. **The lesson is the meta-pattern, not the specific path or filter.**

## 2026-04-27 — `git add -A` in non-brain repos masks unrelated index state

Surfaced via 250c696 (Phase 3 morning session). The rule "per-file git add outside /root/.agent" is not enforceable when scripts use `-A`. Two fixes available: enumerate writers in script (preferred — explicit dependency on which files are touched), or pre-commit status check that aborts if more files staged than expected. evening.sh step 2/4 currently uses `git add -A` in /root/docs; INFRA-EVENING-SCOPE TODO captures the patch. Pattern generalizes: any automated script that commits to a shared repo needs explicit scoping, not blanket staging. **Session-level corollary (added same day after second occurrence):** index state can leak even when explicit `git add <file>` is used — if a prior `M` file was already staged, the new `add` joins the existing index. Two collateral commits in one session (250c696 in /root/docs, e604f6b in /root/.agent) both followed this pattern. The defense is `git status --short` immediately *before* the add to surface unexpected staged files, not after the commit. Rule applies regardless of repo (brain or otherwise) and regardless of whether `-A` or per-file `add` is used.

## 2026-04-27 — Log naming is not always date-suffixed

Surfaced when `/opt/copybot/logs/rescore_*2026-04-26*` glob returned empty despite Sunday SCAN-7 successfully writing to `/opt/copybot/logs/rescore.log` (no date in filename). Forensic rule: when looking for "today's log," `ls -lat | head` is more robust than date-globbing because logging conventions vary (date-suffixed, rotated-on-size, append-only with no date marker). Date-globbing carries an assumption about log naming that should be checked first.

## 2026-04-27 — Paper-mode metrics are not live-mode metrics

The Apr 22 sizing-ceiling LESSONS entry computed median bet $0.31 from paper-mode signals; the live floor at activity_monitor.py:1289-1290 is bypassed in paper mode (`if not paper_mode and bet_size < MIN_LIVE_BET_USD`). Conclusions drawn from paper-mode bet distributions about "what live would do" require explicitly applying live-mode logic to the paper records — observed paper bet size != projected live bet size. Pattern: any "what would live behavior be" question against paper-mode data must re-apply live logic at audit time, not assume paper distribution maps to live.

## 2026-04-27 — Sample-slice direction matters

`records[:200]` after `tail -n 10000 file` returns the *oldest* 200 of the sample, not the freshest. For schema-presence checks (e.g. "does this field exist in current records?"), slice from the end (`records[-200:]`). The audit error here was a schema-detection negative ("intent-like keys found: NONE") that turned out to be wrong because the field was added after the oldest 200 records were written.

## 2026-04-27 — Falsification of an old LESSONS entry deserves explicit documentation, not silent supersession

Today's CB-SIZING-CEILING audit demonstrated that the 2026-04-22 thesis ("median bet $0.31, fix didn't propagate") was wrong because of paper-mode artifact. The fix is to document the falsification (memory edit, FALSIFIED.md candidate, or LESSONS amendment) AND link back to downstream priorities that were affected. Three CopyBot priorities were demoted on the basis of the Apr 22 thesis (CB-BACKTEST-TIER3, CB-BACKTEST-RES-TS, CB-FLIP-GATE) — those decisions need re-examination, not just forward correction. Pattern: when a LESSONS entry is falsified, the supersession should be explicit and downstream-traceable, not implicit.

## 2026-04-27 — `min(legacy, resolved)` gating does not catch wallets where both metrics agree but open-book exposure invalidates the agreement

The 0xaa075924e1 case (Apr 27 Phase 3 magnifier audit) shows a wallet passing both pnl metrics with realized_ratio 0.11 — meaning the agreement itself is uninformative because both metrics are blind to the same thing (open-book MTM exposure). Currently-copied T2 wallet, resolved +$604K, MTM -$4.86M, 330 open positions. A future gate variant may need to read profiler_realized_ratio as a confidence weighting, not just min(legacy, resolved) as a magnitude filter. Pattern generalizes: when two metrics agree because they share a blindspot, the agreement does not reduce uncertainty about the underlying thing.

## 2026-04-27 — When the brain explicitly references a file's path elsewhere, that path is data, not assumption fodder

Apr 27 morning session: assumed evening.sh was in /root/ despite WORKSPACE explicitly listing morning.sh:56,62 and evening_updater.py:51,303 in the INFRA-SERVICE-RENAME TODO, all of which run from /root/docs/. Cost: one round-trip for path correction. Pattern: when reasoning about file locations, search the brain for prior path references before assuming — they're usually there.

## 2026-04-28 — Phase 3 min-gating empirically validated on 44-wallet T2 cohort

**Result:** End-to-end validation of CB_BUYHOLD_ATTR PRODUCER+CONSUMER on real
production T2 wallets. 0/44 integrity violations across three independent
dimensions (consumer flag liveness, PRODUCER coverage, routing correctness).

**Quantitative signal:** 42/44 wallets (95.5%) path-A-fail under min(legacy,
resolved) gating. Cohort was selected by Apr 27 audit using min_gating_pnl;
44/44 expected the same outcome the live gate produced on 42, with 2 having
shifted between Apr 27 snapshot and Apr 28 rescore. The cohort-selection
methodology and the gate's actual behavior agree on 95.5% of wallets — strong
validation that the audit logic itself is sound and reusable.

**Qualitative signal:** Leg-delta distribution is the operationally important
finding. Top swings: $276k, $170k, $132k on individual wallets. These are not
"min-gating slightly tightens admission" magnitudes — they're "legacy
net_pnl_pm was systematically wrong about expected risk by hundreds of
thousands of dollars per wallet." Every wallet copy-traded under the legacy
gate carried that gap as silent risk.

**The 2 admits are positive findings, not failures:**
- 0x1df5647487 (wr 0.874, n=2114): genuine outlier; conservative-side gating
  correctly preserves it.
- 0xd054e72b9e: resolved EXCEEDS legacy, so min() picks legacy — admits on the
  conservative leg as designed.

Reframing rule: do not let future analysis pressure invert these into "the 2
we got wrong." They are "the 2 the gate correctly chose to keep."

**Boundary condition reconfirmed:** 0xaa075924e1 was NOT in the cohort despite
carrying $4.86M MTM exposure. Both pnl signals positive ($604k resolved / +$N
legacy) means min-gating, the audit, and the cohort all blind to it by
construction. This is the realized_ratio blindspot from earlier LESSONS, now
empirically reconfirmed by absence rather than admission. The realized_ratio
gate variant remains the right next investigation surface, but is a separate
epistemic project.

**Operational pre-commitment held:** No action on the 42 demote-eligible
wallets until TIER3 cycle completes (~2026-05-12). The pre-commitment was made
under uncertainty about gate correctness; the resolution of that uncertainty
is what makes honoring the commitment cheap, not expensive.

**Methodology lesson:** Pre-specifying the audit script BEFORE seeing the
full-run data prevented ad-hoc storytelling on the output. The four checks
(admission rate, spotlight wallets, routing correctness, drift) were
formulated before any wallet beyond the dry-run was processed. Apply this
pattern to future validation work: encode the questions as assertions, run
them, read the verdict — don't fish.

**Evidence artifact:**
/opt/copybot/data/phase3/validation_run_20260428.jsonl (46 lines: header + 44
records + footer; preserved from rotation-eligible logs/ source).



## 2026-04-28 — TODO drift is measurable: 60% stale rate observed in Tier 1+2 sweep

**Observation:** Tier 1+2 arc verified 5 WORKSPACE items against actual
production state. 3/5 were already shipped or stale; only 2/5 were genuinely
pending work. Stale items: WeatherBot section (bot retired Apr 22), SCAN-7
cron line (service-managed, not actionable), NewsBot datetime.utcnow()
deprecation (fixed Apr 13 in 30105d8), NewsBot 30s burst debounce (shipped
60s in 60b450f).

**Implication:** WORKSPACE-as-source-of-truth has decayed since the
2026-04-24 brain migration. The migration imported items correctly but
didn't sweep done-status. Every subsequent "what's actionable" survey
inherits the staleness — including, ironically, the survey that produced
this lesson.

**Methodology that worked:** Recon-first on every TODO. The pattern
emerged because each "Block N" started with verification rather than
implementation. The first stale item (datetime.utcnow) could have been
written off as one bad memory; the second (burst debounce) made the
pattern visible; by the third we had base-rate evidence.

**Lesson:** Treat any pre-2026-04-24 WORKSPACE item with the same
skepticism applied to memory-derived facts: verify against /opt/{bot}/
code or git log before treating it as actionable. The cost of verification
is ~5 min per item; the cost of skipping it is starting work that's
already done.

**Spawned remediation:** INFRA-WORKSPACE-DRIFT-AUDIT (parked, dedicated
session). Walking every `[ ]` and verifying once is the only durable fix;
the alternative is paying the verification tax forever on every TODO sweep.

**Connection to earlier 2026-04-28 LESSONS entries:** This is the third
"hidden silent failure" surfaced today. First was the Task 0 grep
no-op'ing for 4 days (~95 SCT-AUTO items). Second was the audit-script
schema mismatch returning 0/3077 admission. Third is TODO drift. Common
shape: a process that produces plausible output without doing what it
claims. Defense in all three cases: don't trust positive-looking results
from a workflow until at least one external check has validated the
workflow itself.

## 2026-04-29 — Phase 3 cohort-coverage gap: validation scope ≠ eligible cohort

**Class:** silent-skip in cohort selection produces validation-scope ≠ validation-claim. Same pattern as 2026-04-28 hardening-block confirmation bias finding (output looks clean, claim's reach is narrower than implied).

**Finding.** Phase 3 cohort selection uses `min(legacy, resolved) ≤ 0` to flag demote-eligible wallets. When `net_pnl_pm` (legacy) is None, `min()` is undefined and the wallet is silently skipped — not flagged, not demoted, not analyzed, not logged. Blast-radius probe 2026-04-29 (predicate: `net_pnl_pm IS NULL AND copy_enabled=True AND tier=='T2'`) returned **179 of 286 T2/copy_enabled wallets** (62.6%).

**What is NOT falsified.** The 2026-04-28 entry's "0/44 integrity violations across three independent dimensions" claim holds for the 44 wallets evaluated. The CB_BUYHOLD_ATTR PRODUCER+CONSUMER pipeline is correct on the cases it sees. Gate logic is correct.

**What IS falsified.** The implicit assumption that "44 wallets evaluated" = "eligible cohort." Actual eligible-cohort = 44 evaluated + 179 silent-skipped = 223. Validation scope was **44/223 = 19.7%**, not 44/44 = 100%. The pre-commitment "no demote action until ~May 12" was made on the cohort Phase 3 saw; it does not bind decisions on wallets the selection logic silently excluded — extending it would lock in continued copy of confirmed-bleeding wallets under a rule that was about not over-acting on cohort findings, not under-acting on findings outside the cohort.

**Five wallets surfaced with negative resolved P&L in the null-legacy bucket.** All copy_enabled=True at finding-time:

- 0xbedbc3808d... (-$63,301 resolved; was Apr 28 #3 worst swing +$10.5k → -$121k; profiler-freshness drift to -$63k a separate INFRA-PROFILER-FIELD-FRESHNESS surface)
- 0xb3b50ef66acf4be... (-$16,003 resolved)
- 0xba0d049aa323... (-$9,000 resolved)
- 0xf86c606c8a36... (-$1,148 resolved)
- 0xa6de582c9853... (-$1,117 resolved)

**Action this session.** copy_enabled=False on 0xbedbc3808d... (smoking-gun event from independent surface). Other four left for proper analysis under CB-PHASE3-NULL-LEGACY-BLINDSPOT. Service restarted to pick up watchlist change.

**Pattern to internalize.** Hardening / validation passes that produce numeric claims (X% pass rate, N/M coverage) need explicit denominator audit before the claim is considered tight. "0/44 integrity violations" answers "are the 44 we saw broken." It does not answer "are the 44 we saw the right 44." Two different questions; second is rarely asked because the first looks complete.

**Postscript (same session).** This pattern recurred on the apply-prompt itself: heredoc-quoting bug ($ literals from a quoted heredoc) caught by pre-run scrutiny before any write. Two instances same day, different surfaces. The discipline is general, not Phase-3-specific.

**Cross-references.** Pairs with 2026-04-27 (falsification-supersession on `min(legacy, resolved)` shared-blindspot in 0xaa075924e1) and 2026-04-28 (hardening-block confirmation bias). All three are silent-failure surfaces from the same review cycle.

### 2026-04-29 — Amendment: silent-skip in tooling + narration drift (six instances same day, five surfaces)

The "pattern to internalize" paragraph above predicted this. Within minutes of writing it, the apply step demonstrated it three more times in tooling and once more in narration. All four are in the same family as the Phase 3 null-legacy cohort-skip.

**1. Index-leak (third recurrence).** `git commit -m "WORKSPACE..."` swept up a pre-staged `memory/working/SCOUTBOT_INBOX.md` change as collateral, because `git status` ran but did not gate. Prior recurrences: 250c696 and e604f6b (LESSONS-2026-04-27). Observation without enforcement is not a fix. **Codify:** apply-scripts must abort if any path is staged that is not in the commit's explicit allow-list. Implemented in this session's recovery prompt — three gates: (a) index empty before staging, (b) only the expected path staged after staging, (c) verify that path tracked after add (catches gitignore-eat).

**2. Gitignore-silent-skip on audit trail.** `git add logs/demote_decisions_20260429.jsonl` returned without error and without staging the file, because `/opt/copybot/.gitignore` blanket-excludes `logs/`. The watchlist commit and LESSONS entry both referenced the audit trail; without the commit, those references were local-only. Different surface from the index-leak, same family. **Codify:** audit-trail files require explicit `-f` or must live outside `logs/`. Forward-looking fix: move audit-trail directory out of `logs/` (e.g. `audit/`) so the blanket gitignore stops catching it.

**3. JSON canonicalization noise.** `sports_watchlist.json` semantic change was one record (target wallet copy_enabled and notes); commit diff shows 5446 insertions / 2787 deletions because `json.dump(indent=2)` produced different whitespace than the prior file format. Cosmetic, but real-world consequence: a reviewer skimming the diff cold would miss the one semantic line in 8000+ lines of JSON noise. Today this is fine because we authored the change; in six months reviewing history, it is a problem. **Decision:** file canonicalized to `json.dump(indent=2)` in commit c7c395e; future edits should match. Not re-rolled — re-rolling risks introducing bugs to fix cosmetics.

**4. Hash-narration drift.** A draft of this amendment carried a SHA from an earlier prompt iteration as the prior watchlist commit reference; the actual landed SHA was `c7c395e`. Caught in pre-run review before any write. Different sub-class than the silent-skip-in-tooling instances above: an operation that *will* succeed (commit goes through) carrying reference data that *won't* resolve (`git show <wrong-hash>` → bad object). Same family because the failure is invisible at write-time and only surfaces when a reader tries to follow the reference. **Codify:** apply-scripts that construct commit messages or LESSONS entries with hash references must capture the live SHA at run-time (`$(git rev-parse --short HEAD)` from the relevant repo) rather than substitute a literal carried over from a draft. The script cannot know what the "right" hash is until the prior commit lands; narration written before that point is provisional.

**Family signature.** Cohort-selection silent-skip, heredoc-quoting silent-skip, index-leak silent-skip, gitignore silent-skip, JSON-noise hiding semantic diff, hash-narration drift. **Six instances same day, five surfaces.** Each was an "operation appeared to succeed because the failure was silent" or "narration appeared correct because the cross-check did not run." General defense: every operation that produces a numeric or boolean confirmation needs a second check that what was confirmed is what was intended. Status checks observe; gates enforce. Live data beats narrated data. Use gates and live data.

### 2026-04-29 — Correction: today's Phase 3 cohort-coverage findings were wrong on the core claim

The two prior 2026-04-29 entries (commits 21dd437 and 4ff5de5) and the WORKSPACE TODO they generated (CB-PHASE3-NULL-LEGACY-BLINDSPOT in commit ef0dcd4) are factually wrong on their core finding. Recording the falsification here in full so future sessions don't operate on the wrong premise.

**What was claimed.**
- 179 of 286 T2/copy_enabled wallets were "silently skipped" from Phase 3 cohort selection because `min(legacy, resolved)` is undefined when `net_pnl_pm` is None.
- Phase 3 validation scope was therefore 44/223 = 19.7%, not 44/44 = 100%.
- Pre-commitment "no demote until ~May 12" was cohort-scoped and did not bind silently-skipped wallets.

**What is actually true.**
- `flip_gate_shadow.py` line 232: `legacy_pnl = profile.get("net_pnl_pm", 0) or 0`. The `or 0` defaults None to 0; `min(0, resolved)` is well-defined. No wallet is silently skipped on this basis.
- More importantly, `profile` in that line is the **fresh `compute_profile()` output**, not the watchlist record. Phase 3 reads gate inputs from a freshly-computed profile per wallet at gate time. The watchlist record's `net_pnl_pm` field is not consulted.
- Validation evidence: `validation_run_20260428.jsonl` shows wallet 0xf86c606c8a (which IS in the morning "null-legacy bucket") got `gate_pnl_legacy: 383.68` at gate time. Real positive number. The field was populated from fresh compute, despite the watchlist record having null.
- Phase 3 cohort selection therefore evaluated the eligible cohort correctly. The 44/44 claim stands. Validation scope was not 19.7%.

**What is still true (narrower bug).**
- `update_watchlist()` in `rescore_watchlist.py:432-475` does not copy `net_pnl_pm` from `new_metrics` to the persisted record. The watchlist's `net_pnl_pm` field is universally null across all 286 records and has been for at least 15 days (verified across four backups Apr 15 / Apr 17 / Apr 24 / Apr 29).
- This is real but narrow. The field is null on disk; no current consumer reads the disk value (gate logic does fresh compute). Impact is "watchlist record looks stale to humans inspecting the file" not "downstream logic is broken."
- Whether the field SHOULD be persisted is a separate question — possibly worth fixing for inspection ergonomics, possibly a deliberate design choice (write-once at harvester-add). Investigation deferred.

**0xbedbc3808d demote (commit c7c395e + df95e17) — action stands, rationale text was wrong.**
- Wallet still bleeds: `profiler_resolved_pnl` = -$63,301 is real, Apr 28 #3 worst swing event is real, copy_enabled=False is the right state.
- The rationale text ("silent-skip in cohort selection") is wrong. The wallet was eligible for Phase 3 cohort selection on the same terms as any other; it just wasn't flagged because it didn't meet the demote threshold under the cohort criteria as Phase 3 ran them.
- Demote-decision audit jsonl gets a correction appended (separate commit on copybot repo).

**Pattern to internalize: trace-the-consumer-not-just-the-producer.**
- The morning's investigation traced backward from "watchlist field is null" to "rescore doesn't write it" and stopped. It never traced forward to ask "does any consumer actually read this null value?" — which would have surfaced the fresh-compute path and falsified the cohort-skip framing before any LESSONS commit.
- Bugs in producer code don't matter if no consumer reads the produced field. Conversely, bugs hypothesized in consumer behavior require tracing the actual consumer code, not assuming that the producer's output shape determines the consumer's behavior.
- General defense: any claim about data-flow, cohort selection, or gate logic requires an end-to-end trace (input → producer → storage → consumer → output) before it goes into LESSONS. Partial trace + plausible synthesis is exactly the failure mode here.

**Family signature update.**
- This is the second major falsification of the day's work-in-progress claims (after the morning's silent-skip-in-tooling family). Different family — that was "tooling silently failed"; this is "narrated reasoning was wrong."
- Both share a meta-defense: the work needs to test against itself, not just against its own internal consistency. Today the test against actual code (flip_gate_shadow.py:232) falsified ~5h of synthesis.
- Worth naming the family: **"stacked-inference falsification"** — confident claim built on partial trace, subsequent reasoning treats it as fact, downstream artifacts compound. Defense: end-to-end trace before commit; mark work-in-progress claims as provisional in LESSONS rather than as findings.

**Brain state action.**
- This entry does NOT delete or rewrite the prior two 2026-04-29 entries. Those remain in git history with their wrong claims; this entry references them and falsifies them. Future readers see the full sequence.
- The CB-PHASE3-NULL-LEGACY-BLINDSPOT TODO body is rewritten in WORKSPACE (separate commit) to reflect the actual narrow bug.
- L20 (CB-MARTINGALE-WATCH-0x8a81855d) is amended: the gating reference to CB-PHASE3-NULL-LEGACY-BLINDSPOT was on the wrong premise; the wallet's demote/threshold decision is on its own merits, not gated on null-legacy resolution.
