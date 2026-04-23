# READ ME FIRST — Claude Chat session-start orientation

**You are the Claude Chat session for Project Helsinki (BigW's bot fleet).**
This file is the first thing you should read at the start of every session.
It lives in the brain and ships with every nightly GitHub push, so it stays
in sync even when other docs don't.

Today is not the day this doc landed — check the `Last updated` field at the
bottom and the GitHub commit history of this repo to know how fresh the
state you are about to read actually is.

---

## 1. Architecture you are inside

```
SERVER (root@204.168.154.197 Hetzner Helsinki HEL1)
├── /root/.agent/               ← THE BRAIN. Source of truth for cross-session state.
│   ├── memory/working/WORKSPACE.md         ← live task state, read first
│   ├── memory/semantic/LESSONS.md          ← cross-session learnings
│   ├── memory/semantic/DECISIONS.md        ← architectural choices + rationale
│   ├── memory/semantic/FALSIFIED.md        ← dead theses, don't re-litigate
│   ├── memory/semantic/STRATEGIC_THREADS_*.md  ← dated strategic snapshots
│   ├── memory/episodic/session_log.jsonl   ← one line per session
│   ├── memory/personal/PREFERENCES.md      ← BigW's working style
│   ├── protocols/permissions.md            ← what needs approval
│   ├── skills/<name>/SKILL.md              ← reusable workflows (morning-check,
│   │                                          evening-close, strategic-review)
│   ├── skills/_index.md                    ← skill registry + trigger phrases
│   └── .git                                ← mirrored to github.com/molensloot77-collab/bot-brain
│
├── /root/docs/                ← operational docs repo. Morning briefs, TODO mirror,
│                                 session logs, evening.sh. Also git-backed. SECONDARY.
│
├── /root/docs/AllBots_TODO_CURRENT.md      ← TODO mirror. Kept for safety but
│                                              WORKSPACE.md is the live state.
├── /root/docs/MORNING_BRIEF.md             ← auto-generated each morning at 03:10 UTC
│                                              by morning_cron; reads current server
│                                              state, not handover docs.
└── /opt/{copybot,weatherbot,…}/            ← bot codebases + their own logs

GITHUB (mirror)
└── github.com/molensloot77-collab/bot-brain  ← PUBLIC repo. Receives nightly push
                                                of /root/.agent/ when BigW runs
                                                evening.sh. This is your read target.

DEPRECATED — do NOT trust
└── /mnt/project/*_CURRENT.md    ← old handover docs. Refreshed only when BigW
                                    manually re-uploads (rare). Often week+-stale.
                                    Known cause of at least three misaligned
                                    sessions on 2026-04-23. AVOID.
```

## 2. Your read path

The brain is mirrored at a **public** GitHub repo. No auth needed.

Base URL: `https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/`

Read at session start in this order — bail out of any file after the first
screen if nothing looks new-to-you:

| Order | Path | Purpose | Typical read cost |
|---|---|---|---|
| 1 | `memory/working/WORKSPACE.md` | Live task state. Read fully. | ~6KB |
| 2 | `memory/personal/PREFERENCES.md` | BigW's working style. Skim if already known. | ~3KB |
| 3 | `memory/semantic/LESSONS.md` | Skim for anything flagged in WORKSPACE. | ~13KB |
| 4 | `memory/semantic/FALSIFIED.md` | Only if a user ask touches a dead thesis. | ~5KB |
| 5 | `memory/episodic/session_log.jsonl` last 3–5 lines | Sequence of recent sessions. | last ~3KB |
| 6 | `memory/semantic/DECISIONS.md` | Only if an architectural question comes up. | ~4KB |
| 7 | `skills/_index.md` | Trigger phrases if considering invoking a skill. | ~2KB |

If a file at the URL returns 404, the nightly push failed or the file was
renamed — fall back to asking BigW to run `cat /root/.agent/<path>` in chat.

## 3. Session-start workflow

claude.ai's `web_fetch` tool only accepts URLs that BigW has explicitly
included in the conversation. To give Claude Chat current brain state at
session start, BigW should paste the following URLs at the start of every
session (or reference this section):

- WORKSPACE: `https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/working/WORKSPACE.md`
- LESSONS: `https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/semantic/LESSONS.md`
- SESSION_LOG: `https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/episodic/session_log.jsonl`

Claude Chat's first action each session: fetch WORKSPACE.md and LESSONS.md
to orient on current state. **Do not trust the MORNING_BRIEF.md
system-prompt snapshot or `/mnt/project/` files as current — they are
week+ stale.**

## 4. Writing to the brain during a session

Claude Chat MUST update `/root/.agent/memory/working/WORKSPACE.md` during
every session that creates new TODOs, resolves existing ones, or records
strategic decisions. The brain is primary; the handover docs in
`/root/docs/` are deprecated (retirement deferred).

If you find yourself writing to `/root/docs/AllBots_TODO_CURRENT.md`
without also updating WORKSPACE.md, you are corrupting the
source-of-truth. During the retirement period, parallel-write both —
**brain first, handover mirror second.**

New lessons worth preserving go into
`/root/.agent/memory/semantic/LESSONS.md` (append-only, read before
non-trivial decisions).

## 5. Cadence — how fresh is the mirror?

- The brain is pushed **only when BigW manually runs `/root/docs/evening.sh`**
  at session close. There is no automated schedule.
- Typical cadence: once per day, usually evening UTC, sometimes skipped.
- **The mirror can therefore be up to ~36 hours behind on any given read.**
  A mid-day Claude Chat session sees the prior evening's state, not today's.
- To check freshness, look at the GitHub repo's latest commit timestamp
  (`https://github.com/molensloot77-collab/bot-brain/commits/main`) or
  WORKSPACE.md's `Last updated` header.
- If BigW says "this was done today" and the mirror doesn't reflect it,
  the mirror is right — today's work hasn't been pushed yet. Don't assume
  the action was skipped; ask BigW to confirm or paste the relevant state.

## 6. The deprecated trap — `/mnt/project/`

The `/mnt/project/*_CURRENT.md` tree that your UI may surface (e.g.
`CopyBot_Handover_CURRENT.md`, `WeatherBot_Handover_CURRENT.md`) is
**deprecated**. It was the previous generation of handover docs, and BigW
stopped refreshing them when the brain + mirror took over. Today's known
incidents (2026-04-23):

- Session A: assumed Path B Gamma repair was still blocked (correct per
  `/mnt/project/`, wrong per brain — fix landed Apr 22). Wasted 20 min.
- Session B: attempted top-20 cull postmortem (already executed Apr 17–18
  per brain; `/mnt/project/` had only the Apr 16 snapshot).
- Session C: mis-attributed Apr 14 cull as "Apr 13 cull" because
  `/mnt/project/` carried that mis-label.

**Rule: when WORKSPACE (brain) and `/mnt/project/` disagree, brain wins.**
No exceptions. If you only have `/mnt/project/`, flag it to BigW and ask
for the brain read, rather than guessing.

## 7. `WORKSPACE.md` vs `AllBots_TODO_CURRENT.md`

Two files track work-in-progress. They look similar and occasionally drift.

- **`/root/.agent/memory/working/WORKSPACE.md` = LIVE STATE.** Authoritative.
  Written by Claude Chat at evening close and by Claude Code during work.
  Brain-mirrored. This is what you should read at session start.
- **`/root/docs/AllBots_TODO_CURRENT.md` = LONG-FORM TODO MIRROR.** Bigger,
  schema'd table. Written mostly by Claude Code sessions (per-task row
  additions) and by the evening updater. Ships in the `/root/docs/` repo,
  not the brain. Claude Code sessions often update this one directly
  without touching WORKSPACE. **That's expected drift**, resolved at the
  next evening close.
- **If the two disagree on a task's status:** the more recently-touched one
  is usually right. For FINISHED work, trust `AllBots_TODO_CURRENT.md`
  (Claude Code rows it immediately). For NEXT-ACTION / current focus,
  trust WORKSPACE.md.
- **Neither of them is the bot's actual running state.** For that you go
  to the server: `systemctl status`, the bot's own log files, signals
  JSONL, etc.

## 8. `MORNING_BRIEF.md` — where it fits

Auto-generated at 03:10 UTC each day by `/root/docs/morning_cron` (reads
live server state — systemctl, signals log, API cost ledger, git log —
not handover docs). Saved to `/root/docs/MORNING_BRIEF.md`. **Not
brain-mirrored.** If BigW opens a morning chat session and you don't have
access to it, ask him to paste it.

MORNING_BRIEF is the best "what happened overnight" summary because it's
generated fresh from live state. If it disagrees with WORKSPACE.md on
anything overnight-related, MORNING_BRIEF wins — WORKSPACE hasn't been
updated with overnight activity yet.

## 9. Standing rules of the road

These come from `PREFERENCES.md`; duplicating the session-critical ones here.

- **Single consolidated prompt blocks to Claude Code.** Never split one
  task's execution across multiple chat messages.
- **Result reporting:** `| HH:MM UTC | RESULT: TASK-N | STATUS: COMPLETE/FAILED | <one line> |`
- **Handover docs are evening-only.** Never update `*_CURRENT.md` mid-session.
- **Don't close a session** until BigW explicitly asks for a close block.
- **Session logs** at `/root/docs/session_logs/YYYYMMDD_CHAT_{BOT}_session.md`.
  Not deferred to `evening.sh`.
- **Four human-only decisions:** deposits, go-live flips, bet size changes,
  new wallet additions. Never automate these, never approve them in your
  draft plan without explicit BigW sign-off.
- **Terse pushback is normal.** "nope", "mama mia", "read the logs again"
  are feedback, not hostility.
- **Give an opinion, not options.** When BigW asks for a recommendation,
  recommend. Option A/B/C without a pick invites pushback.
- **Don't reopen settled questions listed in `FALSIFIED.md`** without new
  evidence.

## 10. What to do if this doc is wrong

The brain is versioned. If something in here doesn't match server reality
(e.g. repo has gone private, a file path has moved, a skill has been
renamed), treat it as a doc bug. Flag to BigW, log it in WORKSPACE.md
under a new `INFRA-BRAIN-DOC-BUG` task, and Claude Code will fix at the
next session.

This doc is a **living summary, not a spec.** Source of truth is always
the current state of the files it points to.

---

*Last updated: 2026-04-23 by Claude Code (CB-CULL-POSTMORTEM session).
Commit hash in bot-brain git log is authoritative.*
