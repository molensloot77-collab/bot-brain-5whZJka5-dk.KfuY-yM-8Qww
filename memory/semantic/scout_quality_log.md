# ScoutBot Quality Log

Append-only longitudinal log of ScoutBot signal quality. One entry per strategic-review or triage run.

Purpose: track kill-rate trend over time to decide whether to build the `scout-quality-audit` skill candidate referenced in LESSONS.md (trigger: 3+ weekly runs >20% kill rate).

Entry format:
## YYYY-MM-DD — <run-type>
- Run size: N items
- Categorization breakdown: ACCEPT-EXECUTE / ACCEPT-DEFER / REJECT-FALSIFIED / REJECT-DUPLICATE / REJECT-LOW-SCORE
- Kill rate (falsified + duplicate + low-score / total): X%
- Notes: any pattern observations, specific falsified categories recurring, data-integrity issues

---

## 2026-04-23 — CLOSEOUT-APR23 overnight triage
- Run size: 30 items
- Categorization: 0 ACCEPT-EXECUTE, 1 ACCEPT-EXECUTE-BLOCKED (data truncation), 21 ACCEPT-DEFER, 8 REJECT-FALSIFIED, 0 REJECT-DUPLICATE, 0 REJECT-LOW-SCORE
- Kill rate: 26.7% (8 falsified / 30 total)
- Notes: First logged data point. LESSONS.md threshold for scout-quality-audit skill build is 3+ weekly runs >20%. This is 1 of 3+. Also: SCT-AUTO-881 address truncation surfaced separately (see INFRA-SCOUT-TRUNCATION in WORKSPACE.md). Triage report: /opt/copybot/logs/sct_auto_triage_20260423.md.
