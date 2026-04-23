# Session start — paste this at the top of every new Claude Chat session

I'm starting a new Claude Chat session. Before generating any plan, fetch current brain state from these URLs and orient on what's actually current:

- WORKSPACE: https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/working/WORKSPACE.md
- LESSONS: https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/semantic/LESSONS.md
- scout_quality_log: https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/semantic/scout_quality_log.md
- research_queue: https://raw.githubusercontent.com/molensloot77-collab/bot-brain/main/memory/semantic/research_queue.md

After fetching, confirm to me:
1. What's the most recent entry in WORKSPACE's "Recent decisions"?
2. What are the active HIGH-priority TODOs right now?
3. Any LESSONS entries added in the last 3 days I should have in mind?

Do NOT trust /mnt/project/ files or the system prompt's morning brief as authoritative — those can lag server state by days. The brain mirror above is current (pushed nightly by evening.sh).

If any fetch fails (permission error, 404, etc.), stop and tell me. Don't proceed on partial state.
