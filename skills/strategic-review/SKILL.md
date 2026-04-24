---
name: strategic-review
version: 2026-04-19
triggers: ["strategic review", "review threads", "what's worth doing", "weekly review"]
tools: [bash, grep]
preconditions:
  - "/root/.agent/memory/working/SCOUTBOT_INBOX.md exists"
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

From `/root/.agent/memory/working/SCOUTBOT_INBOX.md` "## Pending items"
section, extract all rows matching `SCT-AUTO-N` with composite_score ≥ 7.0
AND status == "ACTIVE" AND date ≥ (today − 14 days).

Row format (preserved from pre-2026-04-24 schema):
`| SCT-AUTO-N | bot | description | prio | status | gate | score | date |`
After awk split with `-F'|'`: $2=id, $3=bot, $4=description, $5=prio,
$6=status, $7=gate, $8=score, $9=date, NF=10.

```bash
TODAY=$(date -u +%Y-%m-%d)
CUTOFF=$(date -u -d "14 days ago" +%Y-%m-%d)
grep -E "^\| SCT-AUTO-[0-9]+" /root/.agent/memory/working/SCOUTBOT_INBOX.md | \
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

(Pre-2026-04-24 source was `/root/docs/AllBots_TODO_CURRENT.md`; that file
was retired and its 934 historical IDs are now baked into SCOUTBOT_INBOX.md
header as a dedup roster — they should be skipped here too via the date
cutoff.)

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

Sort surviving threads by `(upside × prob_estimate) / cost` DESCENDING.
Cap at 5 threads. Show the ranking math openly so BigW can re-rank.

**Ordering convention (strict):**
- Rank 1 = highest score (best triage candidate)
- Table rows MUST be in score-descending order
- Any "apparent rank" prose line MUST use the table's row order directly
  — DO NOT re-derive ranking from the same numbers a second time
- For threads with `upside-unknown`, score on `prob / cost` and label
  the row `upside-unknown` in the Notes column. Place these in the
  ranking using their `prob/cost` value but flag clearly.

**Self-check before writing the doc:**
- The thread at rank 1 must have the numerically highest score in the table
- The "Apparent rank by formula" line (if produced) must list thread numbers
  in the same order as the table rows
- If those two checks disagree, STOP and fix before output

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
