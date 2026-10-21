---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可以让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云端代理便会在后台处理：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他事情。

在本系列练习中，你将添加云端代理会遵循的自定义指令，然后分配 GitHub 议题并让它实现任务。你还将审阅并使用自定义代理来调整其工作方式，从代理仪表板监控和引导会话，最后审阅代理创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云端代理会遵循的自定义指令 |
| [2. 云端代理][ex2] | 异步代理 | 将议题分配给 Copilot 云端代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审阅并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求，迭代改进 Copilot 的工作，并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为账户或组织启用 Copilot 云端代理

> [!TIP]
> 没有付费计划？经过验证的学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完成所有学习路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云端代理。对于 Copilot Business 或 Enterprise，管理员必须为组织启用该功能。

> [!NOTE]
> 本学习路径不涉及 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 学习路径](../cli/)或 [VS Code 学习路径](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
