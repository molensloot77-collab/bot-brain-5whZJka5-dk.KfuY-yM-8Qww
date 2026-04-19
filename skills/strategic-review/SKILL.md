---
name: strategic-review
version: 2026-04-19
triggers: ["strategic review", "review threads", "what's worth doing", "weekly review"]
tools: [bash, grep]
preconditions:
  - "/root/docs/AllBots_TODO_CURRENT.md exists"
  - "/root/.agent/memory/semantic/FALSIFIED.md exists"
constraints:
  - "Read-only inputs — no bot code or TODO file is modified"
  - "No decisions made — output is a triage document only"
  - "No backtests run — plausibility estimate only"
  - "No P&L projections promised — upside is order-of-magnitude, bracketed with assumptions"
---

# strategic-review

Weekly triage of ScoutBot SCT-AUTO output. Surfaces 3–5 strategic threads,
cross-references against FALSIFIED.md, estimates cost and upside per thread.
Does not decide. Does not backtest. Does not project P&L.

Purpose: turn 200+ noisy auto-promoted TODO rows into a single page BigW can
read in 5 minutes on a Sunday and use to pick the next week's focus.

## When to run

- Weekly: Sunday afternoon or Monday morning.
- After a big ScoutBot push (>30 new SCT-AUTO items in 48h).
- Before a strategy-pivot decision ("should we build X?") — feeds context, not answer.

## Procedure

### 1. Pull candidate items

From `/root/docs/AllBots_TODO_CURRENT.md`, extract all rows matching
`SCT-AUTO-N` with composite_score ≥ 7.0 AND status == "ACTIVE" AND
date ≥ (today − 14 days).

Row format: `| SCT-AUTO-N | bot | description | prio | status | gate | score | date |`
After awk split with `-F'|'`: $2=id, $3=bot, $4=description, $5=prio,
$6=status, $7=gate, $8=score, $9=date, NF=10.

```bash
TODAY=$(date -u +%Y-%m-%d)
CUTOFF=$(date -u -d "14 days ago" +%Y-%m-%d)
grep -E "^\| SCT-AUTO-[0-9]+" /root/docs/AllBots_TODO_CURRENT.md | \
  awk -F'|' -v cutoff="$CUTOFF" '{
    if (NF != 10) {
      print "WARN: malformed row, skipping: " substr($0,1,80) > "/dev/stderr";
      next
    }
    score=$8+0; status=$6; date=$9;
    gsub(/^ +| +$/,"",status); gsub(/^ +| +$/,"",date);
    if (score >= 7.0 && status == "ACTIVE" && date >= cutoff) print $0
  }'
```

Column-count sanity check is mandatory: silent miscounts on malformed rows
are worse than visible skips. Warnings go to stderr so the triage output
stays clean but skipped rows remain auditable. RETIRED / DONE / DEFERRED /
CLOSED items are excluded — they've already been adjudicated.

### 2. Group by type

Classify each row into one category via keyword match on the description:

- **WALLET_DISCOVERY** — "harvester", "validator", "on-chain", any 0x address
- **EXTERNAL_TOOL** — named repos/tools (OmenAI, Spreadmaker, Polymarket_insider, Homerun, zeitgeist, RaymondDakus, TypeScript CLOB client)
- **MARKET_MECHANISM** — "arbitrage", "rebate", "liquidity", "microstructure", "maker", "spread"
- **DATA_SOURCE** — new APIs, feeds, NOAA, Open-Meteo, Kalshi, unusual data ingestion
- **STRATEGY_PATTERN** — "detection logic", "classification", "regime", "gate", "anomaly", "segmentation"
- **INFRASTRUCTURE** — "monitoring", "dashboard", "alert", "latency", "refresh rate", "dedup"
- **OTHER** — everything else

If a row matches multiple categories, assign to the most specific one
(WALLET_DISCOVERY beats STRATEGY_PATTERN, etc.).

### 3. Identify recurrent themes

For each category with ≥ 3 items, extract the top 3 most-mentioned keywords
or entity names (exclude bot names, stopwords). Collapse items around a
single theme statement.

### 4. FALSIFIED.md cross-reference

For each candidate thread, read FALSIFIED.md section headers. If the thread
is a variation of a falsified thesis, flag it and DO NOT promote it to the
output (note it in a "skipped — falsified" line at the bottom of the doc,
with section reference).

### 5. Write per-thread block

For each surviving thread (cap 5):

- **Pattern**: 1-line theme description
- **Mechanism hypothesis**: 1–2 sentences — why this *might* work,
  synthesized from the SCT-AUTO descriptions
- **Plausible upside**: order-of-magnitude $/day estimate IF calibrated
  against a sibling bot in the same category. If no comparable historical
  bot evidence exists, write "unknown — no comparable bot has run" rather
  than guess. Reserve numerical $/day estimates for cases where a sibling
  bot's P&L provides calibration. When upside is unknown, cost-to-test
  carries the triage weight. Always bracket estimates with assumptions and
  note any partial FALSIFIED.md caveats.
- **Cost to test**: operator hours, capital required, days to paper-validate.
- **Opportunity cost**: 1 line on what alternative use of the same operator
  hours/capital this thread displaces (e.g., "displaces ~10 hrs that would
  otherwise go to WeatherBot live-deployment prep" or "displaces one weekend
  of non-trading work").
- **Falsifiability check**: one specific test that would kill the hypothesis.
- **First step**: smallest action that moves understanding forward (usually
  "read N specific SCT-AUTO items in detail", not "build a bot").

### 6. Sort and cap

Sort surviving threads by `(upside × prob_estimate) / cost`. When upside is
"unknown", rank on `prob_estimate / cost` alone and mark the thread as
"upside-unknown" in the sort column. Cap at 5. Show the ranking math openly
so BigW can re-rank.

### 7. Output file

Write to `/root/.agent/memory/semantic/STRATEGIC_THREADS_YYYY-MM-DD.md`.

Top of document MUST state:

> This is a triage document, not a recommendation. Each thread is a
> plausibility estimate only. No backtest run. No P&L projection.
> Decision and execution remain BigW's.

### 8. Report back

Paste the file contents in chat. That IS the session output.

## What this skill does NOT do

- Backtest any thread
- Recommend which thread to pursue
- Project P&L
- Modify bot code, config, or TODO doc
- Promote SCT-AUTO items to GATED status
- Append to FALSIFIED.md (that's for post-thesis-killed sessions, separate skill)

## Output quality check (run before declaring done)

- [ ] All 5 threads have specific SCT-AUTO-N references (not vague categories)
- [ ] Each thread's mechanism hypothesis is falsifiable
- [ ] No thread is a restatement of something already in FALSIFIED.md
- [ ] Cost-to-test is in operator-hours, not "some time"
- [ ] Upside is bracketed with assumptions, not stated as fact
- [ ] All threads either have numerical upside calibrated against a sibling
      bot, or honestly state "unknown — no comparable bot has run"
- [ ] Every thread has an opportunity cost line
- [ ] Sorting math is visible

If any check fails, iterate before committing.
