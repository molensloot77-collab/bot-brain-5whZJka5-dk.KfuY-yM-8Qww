# /root/.agent/ — Install Notes

Created 2026-04-18 as the single source of truth for Project Helsinki bot operations.
Migrated from userMemories + recent session logs + handover docs.

## Quick install on server

After dragging this folder to `/root/.agent/` via WinSCP:

```bash
cd /root/.agent
git init
git add -A
git commit -m "initial brain scaffold — migrated from userMemories 2026-04-18"

# Optional: push to private repo for off-server backup
# git remote add origin git@github.com:molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww.git
# git push -u origin master
```

Then move `CLAUDE.md` from this folder to `/root/CLAUDE.md`:

```bash
cp /root/.agent/CLAUDE.md /root/CLAUDE.md
```

(Note: Claude Code reads the nearest `CLAUDE.md` walking up the directory tree.
Putting it at `/root/` means every session started from `/root/` or any subdirectory
picks it up. Bot directories can later get their own narrower `CLAUDE.md` if needed.)

## First-session checklist

1. Open Claude Code on the VPS from `/root/`.
2. Run `/init` if not already done — it reads `CLAUDE.md` and builds initial context.
3. Toggle Plan Mode with Shift+Tab. Confirm it says "Plan Mode: on".
4. Type: `morning check` — this should trigger the skill at `skills/morning-check/SKILL.md`.
5. Review the plan it proposes. Approve or correct.

## When things feel wrong

- If Claude Code isn't reading `CLAUDE.md`, check you're running it from `/root/` or below.
- If skills aren't triggering, check trigger phrases in `skills/_index.md` match what you're saying.
- If memory feels stale, check `/root/.agent/memory/working/WORKSPACE.md` — if it's >3 days old, update it or trigger a mini evening-close.
- If Plan Mode is approving things it shouldn't, tighten `protocols/permissions.md`.

## Maintenance

- `git log /root/.agent/memory/` is the operational autobiography. Useful for "when did we decide X".
- Weekly: review `semantic/LESSONS.md` for anything that should become a skill constraint.
- Monthly: read `semantic/FALSIFIED.md` end-to-end — cheap insurance against reopening settled questions.
- When a handover doc at `/root/docs/*_CURRENT.md` and a memory file disagree, memory wins. Update the handover at next evening sync.
