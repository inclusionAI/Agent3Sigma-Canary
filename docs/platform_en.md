# Agent3σ-Canary platform workflow

[简体中文](platform_zh.md) · [Project home](../README.md)

AgentCanary prepares the task environment, runs the target agent, records tool calls and grades the result. Viper-Bench and NARA-Bench share this workflow. Consider a file-sharing task: Viper-Bench can place attacker-controlled content in a document to test whether the agent shares it without authorization. NARA-Bench can give the agent an outdated recipient and check whether it verifies who may receive the file.

![Task configuration, sandbox preparation, agent execution, trajectory collection and scoring](../images/workflow.png)

## Build a task environment with Skill-to-Sandbox

A task file specifies the user request, tools, initial state and grading rubric. Before the run, the platform prepares the files, memory, accounts or service records the task requires. To simulate a new external service, use Skill-to-Sandbox to adapt an existing skill to a local sandbox. The agent keeps using the tool interface, and later calls can observe earlier changes. For example, a file uploaded to a simulated drive should appear in the next directory listing. The evaluator can then check whether the write took place. [Environment guide](eval_env_user_guide_en.md) · [Task format](task_format_en.md) · [Task setup](pre_setup_en.md)

## Select a runtime and execute a task

Build an image by choosing an agent runtime and optionally adding the shared tool layer. The platform supports OpenClaw and its defense plugins, Hermes, NanoClaw, Claude Code and Codex. NARA-Bench tasks declare `required_toolset: safety_research`, which the runner checks against the image tag before execution. The [image build reference](../workflow/README.md) lists the image variants and builder options.

The runner prepares the task materials in an isolated environment and passes the request to the target agent. Adversarial evaluations can use the attack planner shown in the diagram. Multi-turn tasks reuse the same conversation by default. When a task sets `new_session: true`, the runner archives the conversation and starts a new one. Workspace files, installed skills and memory files remain available to subsequent steps. [Viper-Bench tasks](viper_en.md) · [NARA-Bench tasks](nara_en.md)

## Read the trajectory and score

Open the task result under `results/job_*/task_*/` and compare tool calls and their results with the task requirements before interpreting the score. Claude Code and Codex save partial transcripts during execution, so the last recorded step is available after a timeout. Docker runs can also collect system events with `--tracee` for comparison with the agent transcript. [System trajectory guide](system_trajectory_en.md)

Use `--trajectory-only` to inspect the environment and execution trace. This mode retains automated checks and skips judge models. Tasks that depend on a judge show an ungraded result. Before comparing models, check that the task materials loaded and the run completed. [Run validity criteria](run_validity_criteria.md) · [Leaderboard aggregation](../leaderboard/README.md)
