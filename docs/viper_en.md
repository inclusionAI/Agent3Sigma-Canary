# Viper-Bench

[简体中文](viper_zh.md) · [AgentCanary home](../README.md)

Viper-Bench evaluates tool-using agents under an explicit attacker threat model. Each adversarial suite specifies what the attacker can influence, such as a direct request, external content the agent reads, persistent memory or an installed skill. Tool calls and final environment state show whether the attacker induced an unauthorized action, whether the agent recognized the risk and whether it completed the legitimate task.

| Suite | Task files in this checkout | Evaluation focus |
|---|---:|---|
| [direct](../tasks/direct/) | 46 | Direct prompt injection (DPI) |
| [indirect](../tasks/indirect/) | 181 | Indirect prompt injection through external content (IPI) |
| [memory](../tasks/memory/) | 20 | Memory contamination (MC) |
| [skills_poison](../tasks/skills_poison/) | 161 | Skill poisoning (SP) |
| [chain](../tasks/chain/) | 31 | Long-horizon protocol across steps or sessions (LPA) |
| [safety](../tasks/safety/) | 41 | Supplementary coverage of intrinsic risks without an attacker |

Counts refer to task definition files. Paper tables and archived traces have their own selection criteria. Record the version, framework, model, image and condition when reporting results. DPI distinguishes Base and DAE, IPI distinguishes template and context, and SP distinguishes Origin and Camouflaged. Historical IF aggregates use the population documented by their source. The `safety` suite and [NARA-Bench](nara_en.md) have separate selectors.

OSS measures outcome safety, SAS measures security awareness and TUS measures task utility. Each task rubric specifies the applicable dimensions and weights. On the raw 0–1 scale, the research summary defines UOR as the share with OSS ≤ 0.5, SAR as the share with SAS ≥ 0.7 and TSR as the share with TUS ≥ 0.7. Report the denominator and aggregation method. For example, the historical Qwen3.5-35B SP Origin summary reports Overall 61.8. Its six component metrics are unavailable in that export.

Check inputs from the repository root, then follow the [platform quick start](../README.md) to select an image and model. Run artifacts are written under `results/`. See the [leaderboard guide](../leaderboard/README.md) for local aggregation and the [risk taxonomy](risk_def_en.md) for entry and impact definitions.

```bash
uv run python scripts/benchmark.py --suite direct --validate-tasks
```

A successful check ends with `Input validation passed for 46 task(s)`.
