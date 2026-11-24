# NARA-Bench

[简体中文](nara_zh.md) · [AgentCanary home](../README.md)

NARA-Bench evaluates agent risk during ordinary delegated work without an attacker. For example, an agent updating a report might remember an outdated recipient. The evaluation considers whether it checks current authorization, avoids unintended disclosure and completes the legitimate work.

Six mechanisms span twelve business domains. Each combination contains 25 tasks, for 1,800 total. Domains cover HR, marketing, customer support, project management, data analysis, finance, communication, health, legal, IT operations, document management and software engineering.

| Mechanism | Task directory |
|---|---|
| Short-sighted execution strategy | [myopical_execution_strategy](../tasks/NARA-Bench/myopical_execution_strategy/) |
| Outdated memory | [outdated_memory](../tasks/NARA-Bench/outdated_memory/) |
| Preference instruction | [preference_instruction](../tasks/NARA-Bench/preference_instruction/) |
| Spurious correlation in memory | [spurious_correlation_memory](../tasks/NARA-Bench/spurious_correlation_memory/) |
| Unachievable goal | [unachievable_goal](../tasks/NARA-Bench/unachievable_goal/) |
| Vague instruction | [vague_instruction](../tasks/NARA-Bench/vague_instruction/) |

Each task has a unique `id` with a number from 100001 through 101800. Its definition describes its `risk_trigger` and `risk_outcome`. Files, memory and initial tool state establish the scenario. Directory selectors narrow the suite to a mechanism or domain. The `--suite safety` selector runs the 41 supplementary Viper-Bench tasks that assess risk without an attacker.

From the repository root, validate the full suite or a single domain without starting containers or calling models.

```bash
uv run python scripts/benchmark.py --suite NARA-Bench --validate-tasks
uv run python scripts/benchmark.py --suite NARA-Bench/vague_instruction/finance_and_accounting --validate-tasks
```

The final validation message should report 1,800 tasks for the full suite and 25 for the selected domain.

All tasks require Docker and an image tagged with the `safety_research` toolset for execution. Follow the [platform quick start](../README.md) to configure a model and build the image, then select one task for a trial run.

```bash
export DOCKER_IMAGE="<built-safety_research-image-tag>"
uv run python scripts/benchmark.py --suite NARA-Bench --task-ids task_100001_prepare_partner_webinar --docker --model "<provider>/<model>"
```

All 1,800 tasks use `llm_judge`. The rubrics weight safety outcome at 50%, safety awareness at 30% and task utility at 20%. Report scored coverage by mechanism and domain, identifying timeouts, invalid runs and ungraded tasks. The results viewer includes ungraded runs in the total task count and displays their scores as `N/A`. Only runs with numeric scores contribute to the average score. See [run validity criteria](run_validity_criteria.md).
