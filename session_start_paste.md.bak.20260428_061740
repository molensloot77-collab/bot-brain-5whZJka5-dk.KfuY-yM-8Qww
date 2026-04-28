# Session start — paste this at the top of every new Claude Chat session

I'm starting a new Claude Chat session. Before generating any plan, fetch current brain state from these URLs and orient on what's actually current.

Fetch these (raw.githubusercontent.com with cache-buster query — replace `TIMESTAMP` in each URL with current unix epoch via `$(date +%s)` or equivalent):
- WORKSPACE: https://raw.githubusercontent.com/molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww/main/memory/working/WORKSPACE.md?cb=TIMESTAMP
- LESSONS: https://raw.githubusercontent.com/molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww/main/memory/semantic/LESSONS.md?cb=TIMESTAMP
- scout_quality_log: https://raw.githubusercontent.com/molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww/main/memory/semantic/scout_quality_log.md?cb=TIMESTAMP
- research_queue: https://raw.githubusercontent.com/molensloot77-collab/bot-brain-5whZJka5-dk.KfuY-yM-8Qww/main/memory/semantic/research_queue.md?cb=TIMESTAMP

After fetching each: content arrives directly as text (no base64 decode needed). Confirm freshness by sanity-checking the `Last updated:` line near the top of WORKSPACE.md against today's date.

After fetching, confirm to me:
1. What's the most recent entry in WORKSPACE's "Recent decisions"?
2. What are the active HIGH-priority TODOs right now?
3. Any LESSONS entries added in the last 3 days I should have in mind?

Do NOT trust /mnt/project/ files or the system prompt's morning brief as authoritative — those can lag server state by days. The brain mirror above is current (pushed at end of significant work by evening.sh or by Claude Code during close-out). Use the raw URLs above with a cache-buster query (`?cb=<unix_ts>`); the cache-buster forces a CDN miss and returns the current blob. Note: GitHub Contents API is NOT a reliable cache-bypass — empirically observed serving stale blob SHAs independently of `?ref=main` (see LESSONS 2026-04-28). Raw + cache-buster is the working pattern.

If any fetch fails (permission error, 404, etc.), stop and tell me. Don't proceed on partial state.


---
**Note (added 2026-04-28):** This template was changed from GitHub Contents API URLs to `raw.githubusercontent.com` with a cache-buster query parameter. Contents API was empirically observed to serve stale blob SHAs (returning the Friday 2026-04-24 version of WORKSPACE.md when the actual file had been updated Monday 2026-04-27 and was current at origin/main). Contents API has its own cache layer that does not respond to standard cache-bypass hints. The raw URL + `?cb=<unix_ts>` pattern reliably returns current content. See LESSONS 2026-04-28.
