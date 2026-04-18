---
name: evening-close
version: 2026-04-18
triggers: ["evening close", "session close", "close out", "wrap up"]
tools: [bash, grep, git]
preconditions: ["BigW explicitly requested close"]
constraints: ["BigW must explicitly ask before this runs"]
---

# Evening Close

Goal: capture what happened this session, update memory, commit everything,
let `evening.sh` handle handover docs and Telegram.

## Critical precondition

**BigW must explicitly request close.** Never run this skill proactively.
If this skill triggers on "wrap up" but BigW hasn't said "let's close," stop
and confirm.

## Procedure

1. **Summarize the session in one paragraph.** What was the session goal?
   Was it achieved? What changed? What's the next session's starting state?

2. **Append to episodic memory:**
   ```bash
   cat >> /root/.agent/memory/episodic/session_log.jsonl << 'EOF'
   {"timestamp":"<UTC ISO>","session_id":"YYYYMMDD_CHAT_{BOT}","bot":"<bot>","action":"<summary>","result":"<success|failure|partial|decision>","detail":"<context>","pain_score":<1-10>,"importance":<1-10>,"links":["commit:<hash>"]}
   EOF
   ```

3. **Update WORKSPACE.md:**
   - Move completed items out of "Open tasks"
   - Add new items discovered this session
   - Update "Recent decisions (last 7 days)"
   - Update "Current focus" if it changed

4. **Commit memory changes:**
   ```bash
   cd /root/.agent
   git add memory/
   git commit -m "session close YYYY-MM-DD: <one-line summary>"
   ```

5. **Write session log markdown** (for continuity with pre-migration archive):
   ```bash
   cat > /root/docs/session_logs/$(date -u +%Y%m%d)_CHAT_{BOT}_session.md << 'EOF'
   # Session YYYY-MM-DD {BOT}

   ## Goal
   <one line>

   ## What changed
   - <bullet>
   - <bullet>

   ## Commits
   - <hash> - <subject>

   ## Open for next session
   - <bullet>

   ## Gates touched
   - <bullet>
   EOF
   ```

6. **Do NOT touch handover docs.** `evening.sh` owns `/root/docs/*_CURRENT.md` updates.
   `evening_updater.py` does surgical markdown updates, git commits, sends Telegram.

7. **Confirm evening.sh will run** (cron check if needed):
   ```bash
   crontab -l | grep evening
   ```

## Self-rewrite trigger

- If BigW edits the session log markdown manually after this skill writes it, look at what
  they added. If it recurs in 3+ sessions, add it to step 5's template.
- If a commit is ever made outside `/root/.agent/` or `/root/docs/` during close, that's a
  constraint violation — append to FALSIFIED.md under "session hygiene" and flag this skill
  for review.

## Anti-patterns

- **Don't update `/mnt/project/*.md`.** Those are reference snapshots per DECISIONS.md
  2026-04-11. Evening sync only.
- **Don't restart services.** If a service needs restart, that's a mid-session action, not a
  close action.
- **Don't send Telegram directly.** `evening.sh` → Telegram. One message per evening, not three.
