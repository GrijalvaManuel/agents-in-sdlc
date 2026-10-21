---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，整合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在创建自定义代理，将这些能力整合到定义明确的 QA 流程中。

本课将：

- 探索自定义代理如何使用指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 之前，持续检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调该 QA 流程并提供可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用角色。配置文件描述代理的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

之前使用的自定义项承担不同职责：存储库指令说明团队标准；`quality-checks` 技能打包可重复的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些能力，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 之前，让 Copilot 创建可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查及其必须遵守的边界：

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理之前，先检查其配置，确认 Copilot 捕捉到了预期的 QA 流程和边界，避免代理在只要求验证时意外修改功能：

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需的；`name` 是可选的，但添加后代理会有清晰的显示名称。
3. 阅读代理指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在更改实现代码前询问，并且不会提交更改或创建 PR。
5. 如果生成的配置缺少上述职责或边界，在继续之前要求默认代理修改。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话，再选择 QA，以便使用已有的议题和规划决定：

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能不会出现在可用代理列表中，但仍然存在；上述命令可以启用它。

2. 使用以下提示要求 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理将开始工作。
4. 完成后阅读它提供的报告。

## 总结与下一步

你已在工作流中加入可复用的专用角色并审阅其工作。本课中你：

- 探索了自定义代理如何使用指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理并根据筛选议题审阅其发现。

实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来[将它们合并到功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
