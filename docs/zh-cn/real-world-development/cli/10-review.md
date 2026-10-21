---
title: "课程 10 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，再在 Interactive 模式中审查。
- 使用自定义指令引导代理，然后自定义现有 `quality-checks` 技能并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义代理，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为简化工作坊，这些更改合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并使用新格式报告结果。QA 将这些结果与需求及测试覆盖审查结合起来，再提交 PR。

新增测试应该填补真实缺口；如果 QA 运行后发现无需新增测试，也可能是合理结果。合并前要审阅代码和证据。代码更改后，更新受影响的证据。

## 最佳实践

为 Copilot 提供的上下文和工具会影响它的工作。本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复任务，自定义代理定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前规划方案，使用 **Interactive** 持续参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可以选择较快的模型，复杂任务则选择能力更强的模型。

上下文依然和基础设施同样重要。清楚说明希望构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以试试以下 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调互相独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务交给 Copilot 云端代理。

## 后续步骤

熟练使用工具的最佳方式就是持续实践！可以将其用于生产代码、个人项目，或多年想构建却一直没有动手的小应用。与团队分享经验，也向他人学习。别忘了继续探索文档。

如果要从终端继续扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云端代理工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
