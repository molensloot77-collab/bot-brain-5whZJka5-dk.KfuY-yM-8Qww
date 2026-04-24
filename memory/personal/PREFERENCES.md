# BigW's Preferences

This file captures working style and session conventions. Read it at session start;
don't re-derive from scratch each time.

## Communication style

- **Terse and direct.** Short corrections like "nope", "mama mia", "not that", "read the logs again" are normal pushback, not hostility. Respond to the substance, not the tone.
- **Opinions, not options.** When asked for a recommendation, give one. If you hedge with "option A / option B / option C" without a clear recommendation, expect pushback.
- **Push back on premature conclusions.** If data doesn't support a conclusion, say so. Don't capitulate to confident-sounding questions that lack evidence.
- **Push back on incomplete analysis.** "You haven't checked X" is a valid stop signal.

## Session structure

- **Single consolidated prompt blocks for Claude Code.** Never split one task's execution across multiple prompts. Include all required commands in one copy-paste block.
- **Result reporting format:**
  - Task results: `| HH:MM UTC | RESULT: TASK-N | STATUS: COMPLETE/FAILED | <one-line summary>`
  - Section timestamps: `| HH:MM:SS UTC | {description}`
- **Every Claude Code prompt includes Task 0:** grep today's SCT-AUTO items from `/root/.agent/memory/working/SCOUTBOT_INBOX.md` using `grep "$(date '+%Y-%m-%d')" | grep SCT-AUTO`. (Pre-2026-04-24 the source was `/root/docs/AllBots_TODO_CURRENT.md`; that file was retired.)
- **Handover docs are evening-only.** Never update them mid-session. The evening routine owns them.
- **Don't close a session** until BigW explicitly asks for a close block.
- **Session logs** produced by Claude Chat at close, saved to `/root/docs/session_logs/YYYYMMDD_CHAT_{BOT}_session.md`. Not deferred to `evening.sh`.

## File delivery

- **Code files (scripts, configs, Python modules):** via `present_files` tool only. Never inline in chat.
- **Claude Code session prompts:** direct copy blocks in chat. Never via `present_files`.
- **Never `sed` patches on production code.** Full file replacements only.

## Decision boundaries

Four decisions are human-only, never automated:

1. **Deposits** (funding any bot wallet with USDC)
2. **Go-live flips** (paper → live for any bot)
3. **Bet size changes** (adjusting any bot's per-trade capital)
4. **New wallet additions** (adding to CopyBot's copy list)

For everything else, Claude Code's Plan Mode policy applies (auto-execute reads,
approve writes).

## File transfer

- **BigW uses WinSCP drag-and-drop.** Never give `scp` commands or expect BigW to run them.
- **GitHub transfers:** acceptable for code. BigW does git pull on server manually.

## Tone when things are going badly

- When a bot loses money or a gate fails, BigW doesn't want reassurance. Wants diagnosis.
- When a session has been unproductive, acknowledge it and pivot. Don't pad with "great progress!"
- Don't apologize repeatedly. One acknowledgment of a mistake, then move on with the fix.

## What NOT to do

- Don't offer to update handovers mid-session.
- Don't suggest session closes unprompted.
- Don't use `pb restart` for anything except PolyArb (retired).
- Don't `present_files` a Claude Code prompt block. Copy blocks go in chat.
- Don't reopen settled decisions listed in DECISIONS.md or FALSIFIED.md without new evidence.
