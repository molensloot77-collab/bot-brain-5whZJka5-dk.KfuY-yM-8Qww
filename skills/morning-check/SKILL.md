---
name: morning-check
version: 2026-04-18
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

2. **Bot service health.** For each active bot service:
   ```
   for svc in copybot-sports weatherbot krajekbot newsbot; do
     echo "=== $svc ==="
     systemctl is-active $svc
     journalctl -u $svc --since "1 hour ago" -n 3 --no-pager
   done
   ```

3. **CopyBot quick snapshot** (most active bot, check first):
   - Last paper fill timestamp: `tail -1 /opt/copybot/data/sports_signals.jsonl | python3 -c "import sys,json; d=json.loads(sys.stdin.read()); print(d.get('ts',''), d.get('status',''))"`
   - Paper settled last 24h: W/L/WR + P&L
   - Open positions: `wc -l /opt/copybot/data/open_positions.jsonl`
   - USDC balance (Polygonscan for ground truth if manual transfer suspected)

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

🟢 Services: copybot-sports WeatherBot krajekbot newsbot  (all active)
🔴 Down: [list if any]

CopyBot paper:  last 24h  WwLl (X%)  +$YY.YY  |  Open: N  |  Last fill: HH:MM
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

## Self-rewrite trigger

If BigW asks "why didn't you check X" three times in two weeks, add X to step 8.
If BigW says "too much" three times, cut the lowest-priority step (start with 9, then 5).
