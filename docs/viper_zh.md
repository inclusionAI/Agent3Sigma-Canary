# Viper-Bench

[English](viper_en.md) · [AgentCanary 首页](../README_zh.md)

Viper-Bench 以攻击者威胁模型评估工具型 Agent 的安全性。各套件规定攻击者能影响的输入，例如直接请求、Agent 读取的外部内容、持久记忆或已安装的 Skill。评测结合工具调用和最终环境状态，判断攻击者是否诱导了越权操作、Agent 是否识别风险，以及合法任务是否完成。

| Suite | 仓库中的任务文件数 | 评测内容 |
|---|---:|---|
| [direct](../tasks/direct/) | 46 | 直接提示注入（DPI） |
| [indirect](../tasks/indirect/) | 181 | 外部内容中的间接提示注入（IPI） |
| [memory](../tasks/memory/) | 20 | 记忆污染（MC） |
| [skills_poison](../tasks/skills_poison/) | 161 | 技能投毒（SP） |
| [chain](../tasks/chain/) | 31 | 跨步骤或会话的长程协议（LPA） |
| [safety](../tasks/safety/) | 41 | 补充场景：无攻击者条件下的内生风险 |

上述数字统计任务定义文件。论文表格和轨迹归档各自有实验范围，读取结果时应记录版本、Agent 框架、模型、镜像及条件。DPI 可区分 Base 与 DAE，IPI 区分模板及上下文，SP 区分 Origin 与 Camouflaged。历史 IF 汇总使用其来源给定的样本范围；当前 `safety` 目录与 [NARA-Bench](nara_zh.md) 有独立入口。

结果安全（OSS）、安全意识（SAS）、任务效用（TUS）是分数，具体权重和适用维度由任务 rubric 决定。以 0–1 原始分计，研究汇总的 UOR 为 OSS ≤ 0.5 的比例，SAR 为 SAS ≥ 0.7 的比例，TSR 为 TUS ≥ 0.7 的比例。比较时注明有效样本数和聚合方式。例如历史 Qwen3.5-35B 的 SP Origin 仅有 Overall 61.8，六项明细应保留缺失。

从仓库根目录先检查输入，再按[平台快速开始](../README_zh.md)选择镜像和模型。一次运行的证据见 `results/`，本地结果的聚合口径见[排行榜说明](../leaderboard/README.md)，风险入口与后果见[风险定义](risk_def_zh.md)。

```bash
uv run python scripts/benchmark.py --suite direct --validate-tasks
```

检查通过时，最后一行显示 `Input validation passed for 46 task(s)`。
