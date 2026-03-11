# Stock Picking Pipeline Skill

Reusable OpenClaw skill for restoring and running the A-share stock-picking workflow built around Kokoo, Dodoo, and `daily_stock_analysis`.

## WHY THIS SKILL

- The stock-picking flow already exists in pieces, but it was not packaged as a single reusable skill.
- This skill turns the existing operator process into a stable install-and-restore workflow.
- It keeps the orchestration layer separate from the market-analysis engine so the process can be restored without rebuilding everything from memory.

## HOW TO INSTALL

Natural language template for Codex:

`Read https://raw.githubusercontent.com/sontjer/stock-picking-pipeline-skill/refs/heads/main/skill/SKILL.md, install the skill into ~/.openclaw/skills/stock-picking-pipeline, and verify the required dependencies still exist.`

1. Clone this repository:
```bash
git clone https://github.com/sontjer/stock-picking-pipeline-skill.git
```
2. Copy the skill into OpenClaw:
```bash
mkdir -p ~/.openclaw/skills/stock-picking-pipeline
cp stock-picking-pipeline-skill/skill/SKILL.md ~/.openclaw/skills/stock-picking-pipeline/SKILL.md
```
3. Verify the required base components still exist:
```bash
test -f ~/.openclaw/skills/kokoo-dodoo-pipeline/SKILL.md
test -f /root/codex/daily_stock_analysis/SKILL.md
test -d /root/codex/daily_stock_analysis/strategies
```

## HOW TO USE

Natural language examples:

- `执行选股`
- `运行选股流程`
- `按 stock-picking-pipeline 执行`
- `生成5只股票并同步分析`

## WHAT IT RESTORES

- 5-stock shortlist generation flow
- confirmation gate before GitHub sync
- Dodoo sync handoff contract
- proof requirements for completion

## Repository Layout

```text
skill/
  SKILL.md
```
