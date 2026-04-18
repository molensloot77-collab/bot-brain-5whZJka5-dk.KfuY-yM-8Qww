# Episodic Memory

## Schema

Append-only jsonl. Each line is one session or one significant event. Fields:

```json
{
  "timestamp": "ISO-8601 UTC",
  "session_id": "YYYYMMDD_CHAT_{BOT}" or "YYYYMMDD_CODE_{BOT}",
  "bot": "copybot|weatherbot|krajekbot|newsbot|scoutbot|cross",
  "action": "short description of what happened",
  "result": "success|failure|partial|decision",
  "detail": "longer context, up to 500 chars",
  "pain_score": 1-10,
  "importance": 1-10,
  "links": ["commit:abc123", "file:/opt/copybot/foo.py:42"]
}
```

## Scoring

- **pain_score**: how much damage the event caused. 1 = routine, 10 = catastrophic (e.g., phantom P&L discovered after live capital deployed).
- **importance**: how likely this lesson is to be relevant again. 1 = one-off, 10 = will affect every similar future decision.

## Promotion to semantic

The evening dream cycle promotes entries with `pain_score * importance / age_days >= 5.0`
into `/root/.agent/memory/semantic/LESSONS.md` under a `## Auto-promoted YYYY-MM-DD` header.

Entries older than 90 days with `salience < 2.0` are archived to `snapshots/archive_YYYY-MM-DD.jsonl`.

## Manual entry

```bash
echo '{"timestamp":"'$(date -u +%FT%TZ)'","session_id":"'$(date -u +%Y%m%d)_CHAT_COPYBOT'","bot":"copybot","action":"...","result":"decision","detail":"...","pain_score":5,"importance":7,"links":[]}' >> session_log.jsonl
```
