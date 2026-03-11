---
name: stock-picking-pipeline
description: Reusable A-share stock-picking workflow. Use when the user wants to execute stock picking, generate 5 stock codes, run the Kokoo to Dodoo A2A handoff, sync selected stocks to GitHub, or restore the full stock-picking pipeline after reinstalling skills.
---

# Stock Picking Pipeline

Use this skill for the reproducible stock-picking workflow built around:

- Kokoo primary stock picking
- Dodoo GitHub sync and workflow trigger
- `daily_stock_analysis` as the analysis system of record

## Goal

Provide an install-and-restore-ready workflow for:

1. generating a gated 5-stock shortlist
2. sending a structured A2A handoff from Kokoo to Dodoo
3. syncing selected codes to GitHub
4. triggering downstream analysis
5. returning proof

## Canonical Components

- Pipeline reference: `/root/.openclaw/skills/kokoo-dodoo-pipeline/SKILL.md`
- Analysis system: `/root/codex/daily_stock_analysis`
- Analysis skill source: `/root/codex/daily_stock_analysis/SKILL.md`
- Strategy definitions: `/root/codex/daily_stock_analysis/strategies`
- Reproducible handoff reference: `/root/codex/sessions/2026-03-06_kokoo_dodoo_reproducible_skill.md`

## Default Trigger Phrases

- `执行选股`
- `运行选股流程`
- `按 stock-picking-pipeline 执行`
- `生成5只股票并同步分析`

## Hard Workflow

1. Generate A-share momentum / stock-picking output and reduce it to exactly 5 stock codes.
2. Output stock codes as plain 6-digit CSV.
3. Kokoo must send a structured `sessions_send` payload to Dodoo's main session.
4. Dodoo must execute GitHub sync and workflow trigger from that A2A payload.
5. Require proof fields before claiming success.

## A2A Sync Contract

Kokoo must use `sessions_send` and target:

- `sessionKey: agent:dodoo:main`

Message body must be exactly this shape:

```text
[A2A_STOCK_SYNC]
run_id=<run_id>
codes=<code1,code2,code3,code4,code5>
```

Dodoo must treat `[A2A_STOCK_SYNC]` as a privileged internal trigger and run only:

```bash
python3 /root/clawd/scripts/dodoo_sync_stocklist_workflow.py --codes "<csv_codes>" --run-id "<run_id>"
```

Do not rely on Telegram group mentions for this handoff.

## Proof Contract

The resulting execution must return proof including:

- `run_id`
- `stock_list`
- `latest_workflow_run`
- `proof_path`

If proof is incomplete, fail closed.

## Restore Contract

For install-and-restore use cases, verify these exist before claiming the pipeline is restored:

- `/root/.openclaw/skills/kokoo-dodoo-pipeline/SKILL.md`
- `/root/codex/daily_stock_analysis/SKILL.md`
- `/root/codex/daily_stock_analysis/strategies`

If any are missing, report which component is missing instead of pretending the pipeline is available.

## Scope Boundary

- This skill is about workflow orchestration and reproducibility.
- This skill is not the market-analysis engine itself.
- For detailed stock analysis behavior, read `/root/codex/daily_stock_analysis/SKILL.md`.
- For historical group-trigger behavior and evidence expectations, read `/root/codex/sessions/2026-03-06_kokoo_dodoo_reproducible_skill.md`.
