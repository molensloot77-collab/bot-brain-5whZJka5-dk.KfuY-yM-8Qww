# Session start — paste this at the top of every new Claude Chat session

I'm starting a new Claude Chat session. Before generating any plan, fetch current brain state from these URLs and orient on what's actually current.

Fetch these (GitHub Contents API — cache-bypassed; base64 content in `.content`, decode):
- WORKSPACE: https://api.github.com/repos/molensloot77-collab/bot-brain/contents/memory/working/WORKSPACE.md
- LESSONS: https://api.github.com/repos/molensloot77-collab/bot-brain/contents/memory/semantic/LESSONS.md
- scout_quality_log: https://api.github.com/repos/molensloot77-collab/bot-brain/contents/memory/semantic/scout_quality_log.md
- research_queue: https://api.github.com/repos/molensloot77-collab/bot-brain/contents/memory/semantic/research_queue.md

After fetching each: decode the base64 `.content` field. Log the `.sha` field (commit-tree SHA for that file) so freshness is traceable. If the Contents API rate-limits (60/hr unauthenticated), fall back to `https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/<same-path>` for that specific file — but expect up to ~5 min staleness on the raw fallback.

After fetching, confirm to me:
1. What's the most recent entry in WORKSPACE's "Recent decisions"?
2. What are the active HIGH-priority TODOs right now?
3. Any LESSONS entries added in the last 3 days I should have in mind?

Do NOT trust /mnt/project/ files or the system prompt's morning brief as authoritative — those can lag server state by days. The brain mirror above is current (pushed at end of significant work by evening.sh or by Claude Code during close-out). Use the GitHub Contents API URLs above, NOT raw.githubusercontent.com — the API is cache-bypassed; raw has a ~5 min edge cache that bites immediately after a push.

If any fetch fails (permission error, 404, etc.), stop and tell me. Don't proceed on partial state.
