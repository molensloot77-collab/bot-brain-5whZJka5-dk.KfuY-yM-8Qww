# sports_signals.jsonl — schema reference

**Source:** `/opt/copybot/data/sports_signals.jsonl`
**Verified:** 2026-05-02 against frozen snapshot `/tmp/sports_signals_b1.jsonl` (n=42,353 events)

## Field name

The event-type field is named **`event`** — NOT `event_type`.
`event_type` is absent from the corpus (0 / 42,353 events). Using it
returns None and silently skips every event in scans that don't error
on missing keys.

## Universal envelope

These five fields appear in 100% of events:

- `ts` — event timestamp (use this for `event=='RESOLVED'` settled-at;
  use `placed_at` if present for SIGNAL-emitted-at)
- `event` — event-type bucket (see types below)
- `wallet` — source wallet address
- `condition_id` — Polymarket condition id
- `title` — market title

## Event types observed (39-day window)

| event                | order_id present | notes |
|----------------------|------------------|-------|
| SIGNAL               | yes              | placed bet (PENDING) |
| CROWDED_SKIP         | yes              | consensus filter |
| SUB_MIN_BET          | yes              | below sizing floor |
| RECENT_SIGNAL_SKIP   | no               | TTL dedup, pre-order |
| CONTRADICT_SKIP      | no               | RULE 22, pre-order; carries `our_outcome`, `open_outcome`, `open_order_id` |
| RESOLVED             | yes              | settled; carries `won` and `pnl` (only event with these) |
| FILL_GAP_REJECT      | no               | slippage cap, pre-order |
| CANCELLED_TIMEOUT    | yes              | placed but never filled |
| SHADOW_DAILY_LIMIT   | yes              | daily cap |
| DEDUP_SKIP           | no               | consensus dedup, pre-order |
| RULE6_TITLE_SKIP     | yes              | rule 6 |

`order_id` is unique within event-type when present (zero collisions across
16,726 SIGNAL and 2,124 RESOLVED events on 2026-05-02 snapshot).

## Status field

- `LIVE` — present pre-Apr-16 (live-mode data)
- `PAPER` — present Apr-16 onwards (paper-mode-only operation)
- absent on the four pre-order rejects (RECENT_SIGNAL_SKIP, CONTRADICT_SKIP,
  FILL_GAP_REJECT, DEDUP_SKIP)

Mode-flip date in data: **2026-04-16** (mixed transition day on 04-15:
421 LIVE + 540 PAPER). WORKSPACE narrative said Apr-17 — off by 1-2 days.

## Filing rationale

Three rounds of analytics in 2026-05-02 session used `event_type` (wrong)
and produced results that appeared correct due to a translation layer at
the workflow level. This reference note short-circuits future occurrences.
