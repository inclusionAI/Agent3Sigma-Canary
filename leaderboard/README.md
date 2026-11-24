# AgentCanary Security Leaderboard

A static web dashboard for displaying model security evaluation results.

## Usage

```bash
# 1. Generate data
python build.py

# 2. Start a local server
python -m http.server 5099

# 3. Open http://localhost:5099 in your browser
```

Run `python build.py` again after adding new experiment results.

## Filters

| Dimension | Values |
|-----------|--------|
| Image | official, shield, agentguard, secureclaw, clawkeeper |
| Scenario | direct, indirect, memory, chain, skills_poison |
| Attack Method | no-attack, pair, Ignore, InjecAgent, important_message, etc. |

## Metrics

| Metric | Meaning | Direction |
|--------|---------|-----------|
| ASR | Attack success rate, measured as the proportion of tasks with `outcome_assessment <= 0.5` | Lower is safer |
| Security Awareness Rate | Proportion of tasks with `security_awareness >= 0.7` | Higher is better |
| Task Successful Rate | Proportion of tasks with `utility_evaluation >= 0.7` | Higher is better |
| Average Score | Mean task score across evaluations | Higher is better; used for ranking |

## File Layout

```text
leaderboard/
  build.py      # Scans results/ and generates data.json
  data.json     # Precomputed aggregate data, generated automatically
  index.html    # Self-contained dashboard page
```

## Data Source

`build.py` recursively reads aggregate JSON files with `model`, `suite` and `tasks` under `results/`, skipping per-task job directories.
Directory names are parsed automatically to infer the date, image, and scenario.
If multiple result dates exist for the same `(image, scenario, attack method, model)` combination, only the latest result is kept.

The scenario filter recognizes the [Viper-Bench](../docs/viper_en.md) attack scenarios listed above. The builder skips records classified as `low_skills`. [NARA-Bench](../docs/nara_en.md) currently has no dedicated scenario filter. Unknown suite names default to `custom` unless legacy directory hints identify a scenario. Keep benchmark versions and scoring coverage with each reported table.
