# Skills Registry

Read this first. Full `SKILL.md` files only load when their triggers match the
current task. When in doubt, ask.

## morning-check
Single-screen status of all active bots. Read-only.
**Triggers:** "morning check", "morning brief", "status", "where are we"

## evening-close
Capture session, update memory, commit. **Requires BigW's explicit close request.**
**Triggers:** "evening close", "session close", "close out", "wrap up"

## gate-eval
Evaluate a paper gate against its pre-committed metrics. (Placeholder — to be written
when the first gate evaluation session uses it; the shape will emerge from the real workflow.)
**Triggers:** "gate", "evaluate gate", "gate eval", "go/no-go"

## Adding a new skill

When a workflow has been repeated 3+ times with the same structure, migrate it here:

1. `mkdir /root/.agent/skills/<name>/`
2. Write `SKILL.md` with YAML frontmatter (name, version, triggers, tools, preconditions, constraints)
3. Add an entry to this file
4. Test by saying the trigger phrase — Claude Code should find and use it
5. Commit: `cd /root/.agent && git add skills/ && git commit -m "skill: added <name>"`
