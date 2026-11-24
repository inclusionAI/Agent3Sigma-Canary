# Agent3σ-Canary 平台工作流

[English](platform_en.md) · [项目首页](../README_zh.md)

AgentCanary 负责准备任务环境、运行被测 Agent、记录工具调用和评估结果。Viper-Bench 和 NARA-Bench 共用这套流程，区别在于任务怎样引入风险。例如，同样是共享云盘文件，Viper-Bench 可以在文档中放入攻击者控制的内容，观察 Agent 是否越权共享；NARA-Bench 则可以让 Agent 面对一条过时的收件人记录，观察它是否核对了当前授权。

![从任务配置、沙箱准备和执行到轨迹采集与评分](../images/workflow_zh.png)

## 用 Skill-to-Sandbox 构建任务环境

任务文件描述用户请求、需要的工具、初始状态和评分规则。运行前，平台按任务准备工作区文件、记忆、账号或服务记录。要模拟新的外部服务，可以用 Skill-to-Sandbox 把现有 Skill 适配到本地沙箱，保留 Agent 使用的工具接口，并在沙箱中保存操作产生的状态。例如，上传文件后再次列出云盘内容，应该能看到新文件。这样，评测就能核对一次写入是否真正发生。[环境构建指南](eval_env_user_guide_zh.md) · [任务格式](task_format_zh.md) · [任务预置](pre_setup_zh.md)

## 选择运行时并执行任务

构建镜像时先选 Agent 运行时，再按需加装共享工具层。平台支持 OpenClaw 及其防御插件、Hermes、NanoClaw、Claude Code 和 Codex。NARA-Bench 任务声明了 `required_toolset: safety_research`，运行器会在执行前检查镜像标签。具体镜像名称和构建参数见[镜像构建参考](../workflow/README.md)。

运行器把任务所需材料放入隔离环境，再将请求交给目标 Agent。图中的攻击规划器用于对抗性评测。多轮任务默认沿用同一会话；任务声明 `new_session: true` 时，运行器归档上一段对话并开启新会话，工作区文件、已安装的技能和记忆文件仍可供后续步骤使用。[Viper-Bench 任务](viper_zh.md) · [NARA-Bench 任务](nara_zh.md)

## 阅读轨迹与评分

先在 `results/job_*/task_*/` 查看任务结果，将 Agent 的工具调用和返回值与任务要求对照，再看评分。Claude Code 和 Codex 在执行途中保存部分轨迹，即使超时也可以检查最后一步。Docker 模式下还可用 `--tracee` 采集系统事件，与 Agent 的操作记录对照。 [系统轨迹采集指南](system_trajectory_zh.md)

`--trajectory-only` 可用来检查环境和轨迹：它保留自动检查并跳过裁判模型。依赖裁判的任务会显示为未评分。正式比较模型前，先确认任务材料正确加载，并核对执行是否完整。[运行有效性标准](run_validity_criteria.md) · [排行榜汇总说明](../leaderboard/README.md)
