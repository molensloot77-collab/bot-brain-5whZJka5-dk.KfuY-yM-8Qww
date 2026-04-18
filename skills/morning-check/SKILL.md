---
name: morning-check
version: 2026-04-18b
triggers: ["morning check", "morning brief", "status", "where are we"]
tools: [bash, grep]
preconditions: []
constraints: ["read-only", "no service restarts"]
---

# Morning Check

Goal: give BigW a single-screen status of all active bots in under 60 seconds.
No prose explanations until asked — numbers first, context second.

## Procedure

1. **Time and day.** Print current UTC and local (Weert/CET).

2. **Bot service health.** Core services must be active; sibling services
   (dashboards, watchdogs, settlers) are checked but do not fail the brief
   if they are down — only note them under `🟡 Siblings`.
   ```
   CORE="copybot-sports krajekbot weatherbot-scanner newsbot-pipeline"
   SIBLINGS="copybot-dashboard copybot-stream copybot-watchdog polybot-harvester \
             krajekbot-dashboard krajekbot-dashboard-gen krajekbot-settler \
             weatherbot-dashboard weatherbot-watchdog \
             newsbot-dashboard scoutbot-dashboard"
   for svc in $CORE;     do echo "CORE  $svc: $(systemctl is-active $svc)"; done
   for svc in $SIBLINGS; do echo "SIB   $svc: $(systemctl is-active $svc)"; done
   # Only journalctl -u <svc> --since "1 hour ago" -n 3 if a CORE svc is not active.
   ```

3. **CopyBot snapshot** (most active; check first). Source of truth is
   `/opt/copybot/data/sports_signals.jsonl` — append-only log where
   `event:"SIGNAL"` records are fills and `event:"RESOLVED"` records are
   settlements carrying `won` (bool) and `pnl` (float USD). There is no
   separate settled file. The 02:30 UTC `outcome_harvester_*.log` is the
   wallet-leaderboard scanner — ignore it for paper P&L. USDC balance:
   check Polygonscan only if manual transfer suspected.

Run this block (kept at column 0 so the heredoc terminator `PY` parses
cleanly in both shell and the SKILL verifier):

```
python3 - <<'PY'
import json
from datetime import datetime, timezone, timedelta
cutoff = datetime.now(timezone.utc) - timedelta(hours=24)
fills = settled = wins = 0
pnl = 0.0
last_fill_ts = last_resolve_ts = ""
with open('/opt/copybot/data/sports_signals.jsonl') as f:
    for line in f:
        try:
            d = json.loads(line)
        except Exception:
            continue
        ts_str = d.get('ts') or d.get('timestamp') or ""
        try:
            ts = datetime.fromisoformat(ts_str.replace('Z', '+00:00'))
        except Exception:
            continue
        if ts < cutoff:
            continue
        ev = d.get('event')
        if ev == 'SIGNAL' and d.get('status') == 'PAPER':
            fills += 1
            if ts_str > last_fill_ts:
                last_fill_ts = ts_str
        elif ev == 'RESOLVED':
            settled += 1
            if d.get('won'):
                wins += 1
            pnl += d.get('pnl', 0) or 0
            if ts_str > last_resolve_ts:
                last_resolve_ts = ts_str
wr = 100 * wins / settled if settled else 0
print(f"fills_24h={fills}  settled_24h={settled}  wins={wins}  losses={settled-wins}  WR={wr:.1f}%  pnl=${pnl:+.2f}")
print(f"last_fill={last_fill_ts}  last_resolve={last_resolve_ts}")
PY
wc -l /opt/copybot/data/sports_positions.json   # open-position map (by order_id)
```

4. **KrajekBot re-paper progress:**
   - Trade count since last restart
   - v4.6 target remaining (300 minimum)
   - Last regime flag

5. **WeatherBot WB-28 verification days elapsed:**
   - Days since WB-28 filter activated
   - Gate: Apr 22 (14-day verification completes)

6. **NewsBot paper gate:**
   - Signals today: `grep "$(date -u +%F)" /opt/newsbot/data/signals.jsonl | wc -l`
   - Cost today: `grep "$(date -u +%F)" /opt/newsbot/logs/classifier.log | grep -i cost`
   - Settled count (May 7 gate blocker)

7. **ScoutBot overnight priority items:**
   - `grep "$(date -u +%F)" /root/docs/AllBots_TODO_CURRENT.md | grep SCT-AUTO | head -10`

8. **Cross-bot blockers due today:**
   - `grep "$(date -u +%F)" /root/.agent/memory/working/WORKSPACE.md`
   - Any gate dated today

9. **Overnight alerts:**
   - `tail -20 /var/log/syslog | grep -iE "error|fail|kill"` (optional, only if morning check feels off)

## Output format

```
===== Morning Brief — YYYY-MM-DD HH:MM UTC =====

🟢 Core:     copybot-sports  krajekbot  weatherbot-scanner  newsbot-pipeline
🟡 Siblings: [list only those NOT active]
🔴 Down:     [core services not active]

CopyBot paper:  24h  fills=NNN  settled=W/L (WR%)  pnl=$±X.XX  |  Open: N  |  Last fill: HH:MM
KrajekBot:      N trades since restart  |  WR Z%  |  Target: 300 (remaining: M)
WeatherBot:     Day K/14 WB-28 verification  |  Filter pass rate: X%
NewsBot:        N signals today  |  $0.XX spent  |  Settled: 0/121 (gate blocker)
ScoutBot:       K priority items overnight

🎯 Today's gates: [list from WORKSPACE.md]
⚠️  Blockers: [list if any]
```

## What NOT to do

- Don't propose actions. Morning check is status only.
- Don't restart anything (constraint: read-only).
- Don't update WORKSPACE.md from this skill — that's explicit.
- If numbers surprise you, report them flatly. Don't speculate on cause until asked.
- Don't confuse the leaderboard outcome_harvester (02:30 UTC, shadow_outcomes.jsonl, wallet-scanning) with paper settlement events (inline in sports_signals.jsonl, written continuously by copybot-sports).

## Self-rewrite trigger

If BigW asks "why didn't you check X" three times in two weeks, add X to step 8.
If BigW says "too much" three times, cut the lowest-priority step (start with 9, then 5).
