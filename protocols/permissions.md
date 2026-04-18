# Permissions

This file governs Plan Mode behavior. Claude Code reads this before every
tool call that isn't obviously read-only.

## Always allowed (no approval, auto-execute)

### Read-only system inspection
- `cat`, `head`, `tail`, `less`, `grep`, `wc -l`, `find`, `ls`
- `systemctl status <service>` (NOT `restart` or `stop`)
- `journalctl -u <service> -n <N>`
- `git log`, `git status`, `git diff` (no commit)
- `curl` to internal endpoints (localhost, 204.168.154.197)
- `ps`, `top` (non-interactive)
- Python one-liners that read files and print (no writes)

### Read-only Polymarket / blockchain
- Polymarket CLOB API reads (`/book`, `/markets`, `/trades`)
- Gamma API reads (for discovery only; never for settlement per RULE 21)
- Polygonscan / Etherscan reads
- Chainlink oracle reads

### Memory writes (local, non-production)
- Writes to `/root/.agent/memory/working/WORKSPACE.md`
- Appends to `/root/.agent/memory/episodic/session_log.jsonl`
- Git commits within `/root/.agent/`

## Requires approval

### Code changes
- Any edit to `*.py`, `*.json`, `*.yaml`, `*.toml` under `/opt/*/` or `/root/PM_Krajek/`
- Any edit to `.env` files
- Any edit to `/etc/systemd/system/*.service` or drop-ins
- Any edit to `crontab` or `/etc/cron.*/`
- Any edit to `/root/docs/*_CURRENT.md` (handover docs — evening routine only; mid-session requires explicit approval)

### Service control
- `systemctl restart <service>`
- `systemctl stop <service>`
- `systemctl start <service>`
- `systemctl enable/disable <service>`
- `systemctl daemon-reload`

### Git
- `git commit` in bot directories (`/opt/copybot/`, `/opt/weatherbot/`, etc.)
- `git push` anywhere
- `git reset --hard`, `git rebase`, `git cherry-pick`

### Production writes
- Any write to `*.jsonl` or `*.json` outside `/root/.agent/memory/`
- Any database or state file write
- Any Polymarket CLOB order submission (even in paper mode — paper writes still go to disk)

### Shell power
- `rm` on anything outside `/tmp/` or `/root/.agent/memory/working/`
- `chmod`, `chown`
- `apt install`, `pip install` (use `--break-system-packages` when approved)
- Any command with `sudo`

## Never allowed

- **Touch `/opt/polybot/`** — retired, institutional record only. Frozen.
- **Touch `/opt/hub/`** — hard rule, no exceptions.
- **`pb restart` for anything except PolyArb itself** — and PolyArb is retired, so effectively never.
- **Inline `regime_detector.py` code** in KrajekBot.
- **Deposit USDC to any wallet** — BigW's human-only decision.
- **Flip any bot to live mode** — BigW's human-only decision.
- **Change bet sizes** — BigW's human-only decision.
- **Add wallets to any copy list** — BigW's human-only decision.
- **`sed` patches on production Python code** — use full file replacement via present_files.
- **Grep/sed `maker_bot.py` with ASCII-only tools** — contains UTF-8 emoji, will corrupt the file.
- **Trust `capital_check.py` balance alone** after manual wallet transfers. Cross-verify via Polygonscan.
- **Gamma API as settlement source** — CLOB-first, always (RULE 21).
- **Opposing bets on the same condition_id** — RULE 22, hard constraint.

## Approval escalation

If Plan Mode proposes an action in the "requires approval" category and BigW
approves once, that approval applies only to that specific action. No blanket
approvals across sessions.

If Plan Mode proposes an action in the "never allowed" category, it's a bug
in the skill or plan. Reject and flag the skill for review.
